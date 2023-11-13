# Create a resource attribute from an extractpatterns with set

This example showcases how the collector can collect data from files and send it to Splunk Enterprise. A copy of the raw output is sent to a file in the otelcollector accessible with the command `ls foo/file_export` . The example shows how you can create a resource attributes (resource scope) from a body match 

The example runs as a Docker Compose deployment. The collector can be configured to send logs to Splunk Enterprise.

It creates a pipeline, with its own filelog receiver and transform processor. The transform processor in the log context set a new custom_field resource attributes and sets its value to a string that matches the string `logging1` in the body.

Note that the output shows we created a `kvlistValue` in the structure of the example compare the output with the merge maps examples.

[A source type is a default field that identifies the structure of an event. A source type determines how Splunk Enterprise formats the data during the indexing process.](https://docs.splunk.com/Splexicon:Sourcetype)

The example runs as a Docker Compose deployment. The collector can be configured to send logs to Splunk Enterprise.

It creates three pipelines, each with its own filelog receiver and resource processor. Each resource processor sets a `com.splunk.sourcetype` record attribute to a different value, which are then interpreted by the Splunk HEC exporter as their source type.

Splunk is configured to receive data from the OpenTelemetry Collector using the HTTP Event collector. To learn more about HEC, visit [our guide](https://dev.splunk.com/enterprise/docs/dataapps/httpeventcollector/).

To deploy the example, check out this git repository, open a terminal and in this directory type:
```bash
$> docker-compose up
```

Splunk will become available on port 18000. You can login on [http://localhost:18000](http://localhost:18000) with `admin` and `changeme`.

Once logged in, visit the [search application](http://localhost:18000/en-US/app/search) to see the logs collected by Splunk.

You can query the logs index with `index=logs`.

![](different-sourcetypes.png)

output

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
                            "kvlistValue": {
                                "values": [
                                    {
                                        "key": "firstwordseen",
                                        "value": {
                                            "stringValue": " Nov "
                                        }
                                    }
                                ]
                            }
                        }
                    }
                ]
            },
            "scopeLogs": [
                {
                    "scope": {},
                    "logRecords": [
                        {
                            "observedTimeUnixNano": "1699355825239314306",
                            "body": {
                                "stringValue": "Tue Nov  7 11:17:05 UTC 2023 message of logging1"
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
