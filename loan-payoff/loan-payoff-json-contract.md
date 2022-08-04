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
  "loggingErrorMessage": "if error processing, something to log"
}
```
