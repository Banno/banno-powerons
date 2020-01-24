# Loan payment skip JSON contract
To get things started, the client will send an initial request structured like so:
```json
{
  "rgState": "GETPRELOADDATA",
  "powerOnFileName": "BANNO.LOANPAYMENT.SKIP.V1.POW",
  "userChrList": [],
  "userNumList": [],
  "rgSession": 1
}
```
To which the poweron should respond with:
```json
{
    "available": true,
    "availableStartDate": "8/29/2019",
    "availableEndDate": "12/29/2019",
    "feePerPaymentSkip": "35.00",
    "terms":["strings ","with ", "spaces."],
    "availableLoans": [
      {
        "name":"NEW MOTOR HOME",
        "paymentAmount":"136.50",
        "accountId":"1234",
        "originalPaymentDueDate":"8/30/2019",
        "newPaymentDueDate":"9/30/2019"
      }
    ],
    "ineligibleLoans": [
      {
        "name":"HELOC",
        "accountId":"1234",
        "ineligibilityReason":"Insufficient time since loan was opened"
      }
    ],
    "availableShares":[
      {
        "name":"REGULAR SHARES",
        "accountId":"1234",
        "balance":"233.92"
      }
    ]
  }
```
When the client wishes to submit the skip payment request, it will send:
```json
{
  "rgState":"PERFORMSKIPAPAYMENT",
  "powerOnFileName":"BANNO.LOANPAYMENT.SKIP.POWERON",
  "userChrList":[
    {"id": 1, "value": "4CharLoanId, 4CharLoanId..."},
    {"id": 2, "value": "4CharLoanId, 4CharLoanId..."},
    {"id": 3, "value": "4CharLoanId, 4CharLoanId..."},
    {"id": 4, "value": "4CharLoanId, 4CharLoanId..."},
    {"id": 5, "value": "selectedShareId"}],
  "userNumList":[],
  "rgSession":1
}
```
If the request is successful, the poweron should respond with:
```json
{
  "loansWithPaymentsSkipped": [
    {
      "name":"NEW MOTOR HOME",
      "accountId":"1234",
      "paymentAmount":"$136.50",
      "nextDueDate":"9/29/2019",
      "feePaid":"35.00",
      "ShareDebited":"REGULAR SHARES"
    }
  ]
}
```
### Errors
Any and all errors should be conveyed via the following structure:
```json
{
  "clientErrorMessage":"if error processing, a user friendly error message (unused currently)",
  "loggingErrorMessage":"if error processing, something to log"
}
```

