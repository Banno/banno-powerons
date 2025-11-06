# Loan Payoff V2 JSON contract

## GETPRELOADDATA state

### UX Request (GETPRELOADDATA)

```json
{
  "rgState": "GETPRELOADDATA",
  "powerOnFilename": "BANNO.LOAN.PAYOFF.V2.POW",
  "userCharList": [
    { "id": 1, "value": "0123456789" } //10-digit member number
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1]: Member number associated with the selected account (10 digits)

### PowerOn Response (GETPRELOADDATA)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.LOAN.PAYOFF.V2.POW",
        "version": "0.1.0",
        "lastModDate": "01/31/24 16:00 MT",
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2024",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "loanEligible": true,
    "maxDays": 30,
    "disclaimerText": ["FI provided disclaimer text."],
    "summaryText": ["FI provided summary text."]
  }
}
```

**Response detail:**

- generalSpecifications: program and system info
  - programInfo:
    - name: PowerOn Name
    - version: PowerOn version
    - lastModDate: Last modification date/time
    - note1: Note 1
    - note2: Note 2
  - systemInfo:
    - systemDate: Current Episys system date in mm/dd/yyyy format
    - slidLength: The length of the share/loan ID the system is currently using (numeric, 2 or 4)
    - memoMode: boolean - true/false. Is the system in memo mode?
- errorCode: Error code if error condition exists
- errorMessage: Error message if error condition exists
- summaryText: If provided, text is displayed on final screen. If not provided, "This payoff amount is only an estimate. Please contact us for an exact payoff amount." is displayed. Up to 40 lines.
- disclaimerText: Disclaimer text

## PERFORMLOANPAYOFFCALC state

### UX Request (PERFORMLOANPAYOFFCALC)

```json
{
  "rgState": "PERFORMLOANPAYOFFCALC",
  "powerOnFilename": "BANNO.LOAN.PAYOFF.V2.POW",
  "userCharList": [
    { "id": 1, "value": "0123456789" }, // 10-digit member number
    { "id": 2, "value": "yyyy-mm-dd" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1]: Member number associated with the selected account (10 digits)
- userCharList[2]: Payoff date in yyyy-mm-dd format

### PowerOn Response (PERFORMLOANPAYOFFCALC)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.LOAN.PAYOFF.V2.POW",
        "version": "0.1.0",
        "lastModDate": "01/31/24 16:00 MT",
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2024",
        "slidLength": 4,
        "memoMode": false
      }
    },
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

**Response detail:**

- generalSpecifications: program and system info
  - programInfo:
    - name: PowerOn Name
    - version: PowerOn version
    - lastModDate: Last modification date/time
    - note1: Note 1
    - note2: Note 2
  - systemInfo:
    - systemDate: Current Episys system date in mm/dd/yyyy format
    - slidLength: The length of the share/loan ID the system is currently using (numeric, 2 or 4)
    - memoMode: boolean - true/false. Is the system in memo mode?
- errorCode: Error code if error condition exists
- errorMessage: Error message if error condition exists
- totalPayoffAmount: Total payoff amount
- payoffDate:
- accountDetails: account details object
  - principalBalance
  - interestType
  - interestRate
  - interestDue
  - dueDate
  - amountPastDueByPayoffDate
  - pastDuePayoffCount
  - lateChargeDue
- associatedCollateral: associated collateral object
  - label: collateral label
  - info: array of info strings
- payoffFees: array of fees
  - feeAmount: fee amount
  - feeDescription: description of fee

## Errors

Any and all errors should be conveyed via the following structure:

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BNOCUST.xxx.TEMPLATE.POW",
        "version": "0.1.0",
        "lastModDate": "01/31/24 16:00 MT",
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2024",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "errorCode": 500,
    "errorMessage": "if error processing, something to log",
    "errorDisplayMessage": ["Optional member display message - see below"]
  }
}
```

- errorCode: Error code generated (numeric)
- errorMessage: Error message - text description of the errorCode
- errorDisplayMessage: **\*\*Optional** An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message. **Must be supported by UX code**

Possible error codes include:

## Error Codes

| Error Code | Logging Error Message        | Modifier                                                                                 | Additional Notes                                                                           |
| ---------- | ---------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| 500        | Config file open/read error  | [configuration file name] open error - [system generated letter file read error message] | This set of errors represents issues with the configuration letter file                    |
|            |                              | [configuration file name] read error - [system generated letter file read error message] |                                                                                            |
| 501        | Config file validation error | Invalid Param Value([parameter name])                                                    |                                                                                            |
| 502        | Duplicate loan type in CFG   | [param] [4-digit loan type]                                                              | Has a duplicate loan type that is already in home, vehicle or secured loan type parameters |
| 503        | Ineligible Loan Type         | [4-digit loan type]                                                                      | This set of errors is for ineligible loans                                                 |
| 504        | Account Warning Found        | [3-digit comma separated account warning list]                                           |                                                                                            |
| 505        | Loan Projection Error        | [system generated PowerOn function LOANPROJECTCALC error message]                        | This set of errors is for loan payoff processing errors                                    |
| 506        | Fee Specfile Error           | [Banno loan payoff fees PowerOn generated error message]                                 |                                                                                            |
