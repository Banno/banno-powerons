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
### Response Messages & Corresponding Logging Error Messages

*See Logging Error section for additional details

| Client Error Message                                                                 | Logging Error Message                                                       | Notes |
| ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| We're sorry we've encountered an error. Please contact your credit union. | No loan types found in CFG Letter file | This error represents issues with the configuration letter file setup |
|| Invalid loan tracking type: 1234 ||
|| Invalid sub source code: 123 ||
|| Invalid other action: 123 | Configuration error when other action is not 0 share fee comment or 1 loan fee comment |
|| Test mode on but no test account(s) defined ||
|| Invalid start/end date(s) ||
|| Error Opening Letterfile [config file name]: [system generated letter file read error message] ||
| We're sorry we've encountered an error. Please contact your credit union. | Invalid loan id: [loan id] | This type of error is a non-configuration file error |
|| Attempt to skip a payment on an ineligible loan: [loan id] ||
|| Invalid fee share id: [share id] ||
|| Loan Record Update Failed. Error: [file maintenance system error message] ||
|| Tracking Record Create Failed. Error: [file maintenance system error message] ||
|| Skip Pay Fee Post Failed. Error=[transaction posting system error message] ||

