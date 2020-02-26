# Withdraw by check JSON contract

To get things started, the client will send an initial request structured like so:

```json
{
  "rgState": "STATESTART",
  "powerOnFileName": "BANNO.WITHDRAW.CHECK.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789S0123"},
    {"id": 2, "value": ""},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

If successful, the poweron should respond with:

```json
{
  "results": {
    "eligible": true,
    "memberAccountNumber": "0123456789S0123",
    "shareLoanDescription": "ADVANCED CHECKING",
    "available": "123456.00",
    "owner": "Julie Jones",
    "address": [
      "Julie Jones",
      "6525 Chancellor Drive",
      "Cedar Falls",
      "IA",
      "50613",
      "ADDRESS 6"
    ],
    "disclaimerText": ["disclaimer ", "lines ", "with ", "spaces."]
  }
}
```

### Errors

Missing or invalid Letterfile:

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "Error Opening Letterfile BANNO.WITHDRAW.CHECK.V1.CFG: No such file or directory"
}
```

Share cannot have withdraw by check

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "Ineligible Share"
}
```

When the client wishes to submit the check withdrawal request, it will send:

```json
{
  "rgState": "PERFORMWITHDRAW",
  "powerOnFileName": "BANNO.WITHDRAW.CHECK.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789S0123"},
    {"id": 2, "value": "2.00"},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}],
  "userNumList": [],
  "rgSession": 1
}
```

If the request is successful, the poweron should respond with:

```json
{
  "results": {
    "success": true,
    "memoMode": false
  }
}
```

### Errors
If the request is not successful, the poweron should respond with:

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "TRANPERFORM Error:+TRANERROR"
}
```

If the request is not successful due to insufficient funds, the poweron should respond with:

```json
{
  "errorCode": 501,
  "loggingErrorMessage": "TRANPERFORM Error:+TRANERROR"
}
```

In the case of insufficient funds, the client may try request again with a lesser amount.

### Error codes
| Code   | Description         |
|--------|---------------------|
| 500    | Generic Error       |
| 501    | Try again           |
