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
    {"id": 2, "value": "selectedShareId"}],
  "userNumList":[],
  "rgSession":1
}
```
- @RGUSERCHR1 - Comma separated list of up to 25 loans being skipped
- @RGUSERCHR2 - Selected share ID for fee

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
  "clientErrorMessage": "if error processing, a user friendly error message",
  "loggingErrorMessage": "if error processing, something to log"
}
```

### Client Error Message
_Returned in the `clientErrorMessage` json property_

This is the error text that is presented to the end user.
The PowerOn returns the following `clientErrorMessage` for the Program running in memo mode error
and can also be soft-coded the configuration file:
```
Nightly processing is underway. We are unable to complete your skip a pay request.
```
Currently the PowerOn code returns the following `clientErrorMessage` for all other errors: 
```
We're sorry we've encountered an error. Please contact your credit union.
```

### Logging Error Messages
_Returned in the `loggingErrorMessage` json property_

| Logging Error Message                                              | Additional Notes (As Needed)                                                                                     |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| No loan types found in CFG Letter file                             | This set of errors represents issues with the configuration letter file setup                                    |
| Invalid loan tracking type: [4-digit tracking type]                |                                                                                                                  |
| Invalid sub source code: [3-digit sub source code]                 |                                                                                                                  |
| Invalid other action: [3-digit other action]                       | Configuration error when OA (other action) CFG file setting is not 0 (share fee comment) or 1 (loan fee comment) |
| Test mode on but no test account(s) defined                        | Configuration error when TNC (test newest changes) CFG file setting is TRUE, but TML (test member list) is blank |
| Invalid start/end date(s)                                          |                                                                                                                  |
| Error Opening Letterfile [config file name]: [sys generated error] |                                                                                                                  |
| Program running in memo mode                                       | Error when memo mode is active                                                                                   |
| Invalid loan id: [loan id]                                         | This set of errors is for skip payment processing errors                                                         |
| Attempt to skip a payment on an ineligible loan: [loan id]         |                                                                                                                  |
| Invalid fee share id: [share id]                                   |                                                                                                                  |
| Loan Record Update Failed. Error: [file maint system error]        |                                                                                                                  |
| Tracking Record Create Failed. Error: [file maint system error]    |                                                                                                                  |
| Skip Pay Fee Post Failed. Error: [tran posting system error]       |                                                                                                                  |
