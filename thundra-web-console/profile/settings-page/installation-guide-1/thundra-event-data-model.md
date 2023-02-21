# Thundra Event Data Model

The fields below are Thundra events represented as JSON objects. For a more detailed explanation, see our Eventbridge [documentation](./).

| Field              | Values                        | Info                                 |
| ------------------ | ----------------------------- | ------------------------------------ |
| severity           | "Critical", "Warning", "Info" | Defines severity of event            |
| summary            | any string value              | Brief summary of event               |
| thresholdOperation | =, <, >, <=, >=               | Threshold checker operator           |
| period             | 1 min - week                  | Threshold checker period             |
| alertPolicyName    | any string                    | Policy name                          |
| query              | any valid query string        | Event query string                   |
| thresholdValue     | any non negative integer      | Event threshold value                |
| alertEventLink     | link string                   | Thundra event link                   |
| resultNumber       | any non negative integer      | Number of occurrence in given period |

Example Thundra event:

```javascript
{
  "version": "0",
  "id": "7a57b5f0-06c1-1743-55eb-990cefb359a6",
  "detail-type": "Thundra-Event",
  "source": "aws.partner/thundra.test/eQ1krlx2-sourceName",
  "account": "**********",
  "time": "2019-11-12T08:13:20Z",
  "region": "eu-west-1",
  "resources": [],
  "detail": {
    "severity": "info",
    "summary": "A thundra alert triggered by Duration policy.",
    "thresholdOperation": "greater than or equal to",
    "period": "15 min",
    "alertPolicyName": "Duration",
    "query": "AVG(Duration) > 10 ORDER BY LastInvocationTime DESC",
    "thresholdValue": "10",
    "alertEventLink": "https://lab.thundra.io/alert-event/alert-event-bea04941-cbd2-41f4-bcfa-7bae40bf77b5",
    "resultNumber": "1"
  }
}
```
