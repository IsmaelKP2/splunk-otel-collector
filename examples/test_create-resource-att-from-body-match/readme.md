# Create a resource attribute from a body match 

This example showcases how the collector can collect data from files and send it to Splunk Enterprise. A copy of the raw output is sent to a file in the otelcollector accessible through `ls foo/filexport` . The example shows how you can create a resource attributes (resource scope) from a body match 

The example runs as a Docker Compose deployment. The collector can be configured to send logs to Splunk Enterprise.

It creates a pipeline, with its own filelog receiver and transform processor. The transform processor in the log context set a new custom_field resource attributes and sets its value to a string that matches the string `logging1` in the body.

Splunk is configured to receive data from the OpenTelemetry Collector using the HTTP Event collector. To learn more about HEC, visit [our guide](https://dev.splunk.com/enterprise/docs/dataapps/httpeventcollector/).

To deploy the example, check out this git repository, open a terminal and in this directory type:
```bash
$> docker-compose up
```

Splunk will become available on port 18000. You can login on [http://localhost:18000](http://localhost:18000) with `admin` and `changeme`.

Once logged in, visit the [search application](http://localhost:18000/en-US/app/search) to see the logs collected by Splunk.

You can query the logs index with `index=logs`.


**foo output**

```
{
    "resourceLogs": [
        {
            "resource": {
                "attributes": [
                    {
                        "key": "com.splunk.sourcetype",
                        "value": {
                            "stringValue": "sourcetype1"
                        }
                    },
                    {
                        "key": "custom_field",
                        "value": {
                            "stringValue": "logging1"
                        }
                    }
                ]
            },
            "scopeLogs": [
                {
                    "scope": {},
                    "logRecords": [
                        {
                            "observedTimeUnixNano": "1699354492327358399",
                            "body": {
                                "stringValue": "Tue Nov  7 10:54:52 UTC 2023 message of logging1"
                            },
                            "attributes": [
                                {
                                    "key": "log.file.name",
                                    "value": {
                                        "stringValue": "file.log"
                                    }
                                }
                            ],
                            "traceId": "",
                            "spanId": ""
                        }
                    ]
                }
            ]
        }
    ]
}
```
