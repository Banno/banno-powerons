# Withdraw by check JSON contract

To get things started, the client will send an initial request structured like so:

```json
{
  "rgState": "STATESTART",
  "powerOnFileName": "BANNO.CHECK.WITHDRAW.V1.POW",
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

Missing or invalid Letterfile (or any generic, non-actionable error):

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "Error Opening Letterfile BANNO.WITHDRAW.CHECK.V1.CFG: No such file or directory"
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
If the request is not successful, the poweron should respond with one of the following errors:

If there was an issue opening or reading the configuration Letter file
```json
{
  "errorCode": 500,
  "loggingErrorMessage": "Error [opening/reading] Letter file: +[error returned]"
}
```

If the request is not successful due to insufficient funds, the poweron should respond with the
following and the client may try request again with a lesser amount:

```json
{
  "errorCode": 501,
  "loggingErrorMessage": "Amount req. ###,##9.99 exceeds avail. ###,##9.99"
}
```
In the case of insufficient funds, the client may try request again with a lesser amount.

Should the program determine that the member's address is insufficient to mail a check, the
response would be as follows:

```json
{
  "errorCode": 502,
  "loggingErrorMessage": "Invalid address"
}
```

If the request is not successful due to an invalid account, loan or share, the poweron would respond with:

```json
{
  "errorCode": 503,
  "loggingErrorMessage": "[Account Not Found/Account Warning Found/Share Not Found/Loan Not Found/Share Warning Found/Loan Warning Found]"
}
```

If the request is not successful due to Reg D Limits, the response will be:
```json
{
  "errorCode": 504,
  "loggingErrorMessage": "TRANPERFORM Error: Reg D Limit"
}
```

If the member attempts to make a WD from a cross account, the poweron will respond with:.

```json
{
  "errorCode": 505,
  "loggingErrorMessage": "Cross Account WD Attempted"
}
```

### Error codes
| Code   | Description              |
|--------|--------------------------|
| 500    | CFG file error           |
| 501    | Insufficient Funds       |
| 502    | Invalid address          |
| 503    | Invalid Acct/loan/share  |
| 504    | Reg D Limit              |
| 505    | X-Acct Error             |
