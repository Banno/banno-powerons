# Loan payoff JSON contract

To get things started, the client will send an initial request structured like so:

```json
{
  "rgState": "GETPRELOADDATA",
  "powerOnFilename": "BANNO.LOAN.PAYOFF.V1.POW",
  "userCharList": [
    { "id": 1, "value": "member-account-number" },
    { "id": 2, "value": "" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

To which the poweron should respond with:

```json
{
  "clientErrorMessage": null,
  "loggingErrorMessage": null,
  "results": {
    "loanEligible": true,
    "maxDays": 30,
    "disclaimerText": "FI provided disclaimer text."
  }
}
```

When the client wishes to submit the loan payoff calculation request, it will send:

```json
{
  "rgState": "PERFORMLOANPAYOFFCALC",
  "powerOnFileName": "BANNO.LOAN.PAYOFF.V1.POW",
  "userCharList": [
    { "id": 1, "value": "memberAccountNumber" },
    { "id": 2, "value": "yyyy-mm-dd" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

If the request is successful, the poweron should respond with:

```json
{
  "clientErrorMessage": "A user friendly error message (configurable by FI)",
  "loggingErrorMessage": "an error message used in logging",
  "results": {
    "totalPayoffAmount": "5,774.60",
    "payoffDate": "yyyy-mm-dd",
    "accountDetails": {
      "principalBalance": "4,814.76",
      "interestType": "Daily Billed 360 (configurable by FI, optional display, maybe remove completely)",
      "interestRate": "3.400% (optional. not supplied for credit cards)",
      "interestDue": "1.43 (not in design spec currently)",
      "dueDate": "yyyy-mm-dd",
      "amountPastDueByPayoffDate": "0.00",
      "pastDuePayoffCount": "0",
      "lateChargeDue": "5.00"
    },
    "associatedCollateral": {
      "label": "Vehicle",
      "info": ["2004", "Pontiac", "Aztec"]
    },
    "payoffFees": [
      { "feeAmount": "20.00", "feeDescription": "Documentation Fee" },
      { "feeAmount": "25.00", "feeDescription": "Processing Fee" }
    ]
  }
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

*All Errors begin with 'We're sorry we've encountered an error. Please contact your credit union.' followed by the Logging Error Message.
 That is the part of the error message that is displayed to the end user.

*See the Modifier section for additional details.
 The Modifier is appended to the main Logging Error Message.

| Logging Error Message                                                                 | Modifier                                                                             | Additional Notes As Needed                    |
| ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| Error Opening Letterfile [config file name] | : [system generated letter file read error message] | This set of errors represents issues with the configuration letter file setup |
| Duplicate loan type in CFG (BLT 1234) || In the CFG boat loan type has a duplicate loan type that is already in home, vehicle or secured loan type parameters. Remove the duplicates to resolve. |
| Duplicate loan type in CFG (VLT 1234) || In the CFG vehicle loan type has a duplicate loan type that is already in home, boat or secured loan type parameters. Remove the duplicates to resolve. |
| Duplicate loan type in CFG (SLT 1234) || In the CFG secured loan type has a duplicate loan type that is already in home, boat or vehicle loan type parameters. Remove the duplicates to resolve. |
| Ineligible Loan Type | : [4-digit loan type] | This set of errors is for ineligible loans |
| Payoff Days more than Maximum allowed |||
| Account Warning Found | : [3-digit comma separated account warning list] ||
| Loan Warning Found | : [3-digit comma separated loan warning list] ||
| Loan Projection Error | : [system generated PowerOn function LOANPROJECTCALC error message] | This set of errors is for loan payoff processing errors |
| Fee Specfile Error | : [Banno loan payoff fees PowerOn generated error message] ||
