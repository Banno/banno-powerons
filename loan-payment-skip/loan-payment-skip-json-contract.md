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
  "feeLoanApplyOption": false,
  "terms": ["strings ", "with ", "spaces."],
  "availableLoans": [
    {
      "name": "NEW MOTOR HOME",
      "paymentAmount": "136.50",
      "accountId": "1234",
      "originalPaymentDueDate": "8/30/2019",
      "newPaymentDueDate": "9/30/2019"
    }
  ],
  "ineligibleLoans": [
    {
      "name": "HELOC",
      "accountId": "1234",
      "ineligibilityReason": "Insufficient time since loan was opened"
    }
  ],
  "availableShares": [
    {
      "name": "REGULAR SHARES",
      "accountId": "1234",
      "balance": "233.92"
    }
  ],
  "programInfo": "BANNO.LOANPAYMENT.SKIP.V1.POW  1.2.0  02/08/24   TESTMODE:FALSE  TESTACCT:FALSE"
}
```

When the client wishes to submit the skip payment request, it will send:

```json
{
  "rgState": "PERFORMSKIPAPAYMENT",
  "powerOnFileName": "BANNO.LOANPAYMENT.SKIP.V1.POW",
  "userChrList": [
    { "id": 1, "value": "4CharLoanId, 4CharLoanId..." },
    { "id": 2, "value": "selectedFeeId" } // 2 or 4-digit share id or "toLoan"
  ],
  "userNumList": [],
  "rgSession": 1
}
```

If the request is successful, the poweron should respond with:

```json
{
  "loansWithPaymentsSkipped": [
    {
      "name": "NEW MOTOR HOME",
      "accountId": "1234",
      "paymentAmount": "$136.50",
      "nextDueDate": "9/29/2019",
      "feePaid": "35.00",
      "ShareDebited": "REGULAR SHARES",
      "loanDebited": "LINE OF CREDIT"
    }
  ]
}
```

### Errors

Any and all errors should be conveyed via the following structure:

```json
{
  "clientErrorMessage": "if error processing, a user friendly error message (unused currently)",
  "loggingErrorMessage": "if error processing, something to log"
}
```

### Response Messages & Corresponding Logging Error Messages

**All Errors begin with 'We're sorry we've encountered an error. Please contact your credit union.' followed by the Logging Error Message.
That is the part of the error message that is displayed to the end user.**

| Logging Error Message                                              | Additional Notes (As Needed)                                                                                     |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| No loan types found in CFG Letter file                             | This set of errors represents issues with the configuration letter file setup                                    |
| Invalid loan tracking type: [4-digit tracking type]                |                                                                                                                  |
| Invalid sub source code: [3-digit sub source code]                 |                                                                                                                  |
| Invalid other action: [3-digit other action]                       | Configuration error when OA (other action) CFG file setting is not 0 (share fee comment) or 1 (loan fee comment) |
| Test mode on but no test account(s) defined                        | Configuration error when TNC (test newest changes) CFG file setting is TRUE, but TML (test member list) is blank |
| Invalid start/end date(s)                                          |                                                                                                                  |
| Error Opening Letterfile [config file name]: [sys generated error] |                                                                                                                  |
| Invalid loan id: [loan id]                                         | This set of errors is for skip payment processing errors                                                         |
| Attempt to skip a payment on an ineligible loan: [loan id]         |                                                                                                                  |
| Invalid fee share id: [share id]                                   |                                                                                                                  |
| Loan Record Update Failed. Error: [file maint system error]        |                                                                                                                  |
| Tracking Record Create Failed. Error: [file maint system error]    |                                                                                                                  |
| Skip Pay Fee Post Failed. Error: [tran posting system error]       |                                                                                                                  |
