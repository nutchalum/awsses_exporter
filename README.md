# AWS SES Exporter

Exporter for AWS SES quotas. `awsses_exporter` will poll all AWS SES regions and report back the max send rate, max send per 24 hours, and current send in last 24 hours. The AWS SES API endpoint `GetSendQuota` is called once per region per call to the `/metrics` endpoint

## Usage
```
$ awsses_exporter --help
usage: collector [<flags>]

Flags:
  -h, --help              Show context-sensitive help (also try --help-long and --help-man).
      --web.listen-address=":9199"
                          Address to listen on for web interface and telemetry.
      --web.telemetry-path="/metrics"
                          Path under which to expose metrics.
      --log.level="info"  Only log messages with the given severity or above. Valid levels: [debug, info, warn, error, fatal]
      --log.format="logger:stderr"
                          Set the log target and format. Example: "logger:syslog?appname=bob&local=7" or "logger:stdout?json=true"
      --version           Show application version.
```


## Build

This easiest way to build and run this utility is with the included docker image: `docker build .`

You can also install it locally with:
```
go get github.com/sysadmind/awsses_exporter
go install github.com/sysadmind/awsses_exporter
```


## Example Metrics
Below is an example of the output of the `/metrics` endpoint
```
# HELP awsses_exporter_max24hoursend The maximum number of emails allowed to be sent in a rolling 24 hours.
# TYPE awsses_exporter_max24hoursend gauge
awsses_exporter_max24hoursend{aws_region="eu-west-1"} 50000
awsses_exporter_max24hoursend{aws_region="us-east-1"} 50000
awsses_exporter_max24hoursend{aws_region="us-west-2"} 50000
# HELP awsses_exporter_maxsendrate The maximum rate of emails allowed to be sent per second.
# TYPE awsses_exporter_maxsendrate gauge
awsses_exporter_maxsendrate{aws_region="eu-west-1"} 14
awsses_exporter_maxsendrate{aws_region="us-east-1"} 14
awsses_exporter_maxsendrate{aws_region="us-west-2"} 14
# HELP awsses_exporter_sentlast24hours The number of emails sent in the last 24 hours.
# TYPE awsses_exporter_sentlast24hours gauge
awsses_exporter_sentlast24hours{aws_region="eu-west-1"} 101
awsses_exporter_sentlast24hours{aws_region="us-east-1"} 505
awsses_exporter_sentlast24hours{aws_region="us-west-2"} 888
```
