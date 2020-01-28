# Withdraw by check JSON contract
To get things started, the client will send an initial request structured like so:
```json
{
  "rgState": "GETPRELOADDATA",
  "powerOnFileName": "BANNO.WITHDRAW.CHECK.V1.POW",
  "userChrList": [],
  "userNumList": [],
  "rgSession": 1
}
```
To which the poweron should respond with:
```json
{
  "accountEligible": true,
  "accountNumber": "0123456789",
  "accountBalance": "$5.00",
  "address": {
    "fullName": "Julie Jones",
    "street": "6525 Chancellor Drive",
    "city": "Cedar Falls",
    "state": "IA",
    "zip": "50613"
  },
  "disclaimerText": ["disclaimer ", "lines ", "with ", "spaces."]
}
```
When the client wishes to submit the check withdrawal request, it will send:
```json
{
  "rgState": "SUBMITWITHDRAWBYCHECKREQUEST",
  "powerOnFileName": "BANNO.WITHDRAW.CHECK.V1.POW",
  "userChrList":[
    {"id": 1, "value": "member-account-number"},
    {"id": 2, "value": "4charLoanId"},
    {"id": 3, "value": "2.00"},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}],
  "userNumList": [],
  "rgSession": 1
}
```
If the request is successful, the poweron should respond with:
```json
{
  "success": true
}
```

### Errors
Any and all errors should be conveyed via the following structure (combined with the responses above):
```json
{
  "clientErrorMessage": "if error processing, a user friendly error message (unused currently)",
  "loggingErrorMessage": "if error processing, something to log"
}
```

