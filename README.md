# fluentd-syslog-to-cloudwatchlogs

## How does this work?

Docker container running Fluentd accepts log messages in Syslog format on the TCP port
specified. Logs are parsed using a grok pattern (for now parsing iptables logs) and 
sent to Cloudwatch Logs using the API key and secret provided.

All config variables passed to container using env variables but could be injected in 
other ways. User account linked to API key should have minimal rights to write logs
and create log streams, similar to this:

`Action: ['logs:createLogStream','logs:PutLogEvents','logs:DescribeLogStreams']`

This container does NOT need to run in AWS (it should be able to run anywhere as long
as it has internet access and can reach AWS)

IPtables logs will be parsed in this example and a GeoIP lookup will be performed
on the source address of the packet that is being logged.

Everything lands in Cloudwatch Logs in a structured format (JSON).

## How do you run this?

Using a docker-compose.yml similar to this:

  fluentd:
    build: ./fluentd-config/
    ports:
      - "6514:6514"
    volumes:
      - "./fluentd-config:/fluentd/etc/"
    environment:
      TCPport: 6514
      AWS_ACCESS_KEY_ID: <KEY_ID_HERE>
      AWS_SECRET_ACCESS_KEY: <KEY_SECRET_HERE>
      AWS_REGION: us-west-2
      LocationTag: ArkansasDatacenter



Edit env variables as needed then run:
`docker-compose -f docker-compose.yml build`
`docker-compose -f docker-compose.yml up -d`


