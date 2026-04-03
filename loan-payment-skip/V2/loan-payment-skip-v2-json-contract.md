# Loan Payment Skip JSON contract

## GETPRELOADDATA state

### UX Request (GETPRELOADDATA)

```json
{
  "rgState": "GETPRELOADDATA",
  "powerOnFilename": "BANNO.LOANPAYMENT.SKIP.V2.POW",
  "userCharList": [],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1]: n/a
- userNumList[1]: n/a

### PowerOn Response (GETPRELOADDATA)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.LOANPAYMENT.SKIP.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/31/26 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2026",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "available": true,
    "availableStartDate": "8/29/2019",
    "availableEndDate": "12/29/2019",
    "feePerPaymentSkip": "35.00",
    "termsMsg": ["strings ", "with ", "spaces."],
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
    - language: 1 = English, 2 = Spanish
    - note1: Note 1
    - note2: Note 2
  - systemInfo:
    - systemDate: Current Episys system date in mm/dd/yyyy format
    - slidLength: The length of the share/loan ID the system is currently using (numeric, 2 or 4)
    - memoMode: boolean - true/false. Is the system in memo mode?
- errorCode: Displays an error code if error condition exists
- errorMessage: Displays an error message if error condition exists
- available
- availableStartDate
- availableEndDate
- feePerPaymentSkip
- termsMsg: array of terms messages
- availableLoans: array of available loans
  - name
  - paymentAmount
  - accountId
  - originalPaymentDueDate
  - newPaymentDueDate
- ineligibleLoans: array of ineligible loans
  - name
  - accountId
  - ineligibilityReason
- availableShares: array of available shares
  - name
  - accountId
  - balance

## PERFORMSKIPAPAYMENT state

Handles the skip payment request

### UX Request (PERFORMSKIPAPAYMENT)

```json
{
  "rgState": "PERFORMSKIPAPAYMENT",
  "powerOnFilename": "BANNO.LOANPAYMENT.SKIP.V2.POW",
  "userChrList": [
    { "id": 1, "value": "4CharLoanId, 4CharLoanId..." },
    { "id": 2, "value": "selectedShareId" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1]: Comma separated list of up to 25 loans being skipped
- userCharList[2]: Selected share ID for fee

### PowerOn Response (PERFORMSKIPAPAYMENT)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.LOANPAYMENT.SKIP.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/31/26 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2026",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "successMsg": ["Custom success message line 1","Custom success message line 2"],
    "loansWithPaymentsSkipped": [
      {
        "name": "NEW MOTOR HOME",
        "accountId": "1234",
        "paymentAmount": "$136.50",
        "nextDueDate": "9/29/2019",
        "feePaid": "35.00",
        "ShareDebited": "REGULAR SHARES"
      }
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
    - language: 1 = English, 2 = Spanish
    - note1: Note 1
    - note2: Note 2
  - systemInfo:
    - systemDate: Current Episys system date in mm/dd/yyyy format
    - slidLength: The length of the share/loan ID the system is currently using (numeric, 2 or 4)
    - memoMode: boolean - true/false. Is the system in memo mode?
- errorCode: Displays an error code if error condition exists
- errorMessage: Displays an error message if error condition exists
- successMsg: array of success messages
- loansWithPaymentsSkipped: array of loans that had payments skipped
  - name
  - accountId
  - paymentAmount
  - nextDueDate
  - feePaid
  - ShareDebited

## Errors

Any and all errors should be conveyed via the following structure:

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.LOANPAYMENT.SKIP.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/31/26 16:00 MT",
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2026",
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

## Error Codes

| Error Code | Logging Error Message                           | Modifier                                                                                 |
| ---------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------- |
| 500        | System is in memo mode                          |                                                                                          |
| 501        | Config file open/read error                     | [configuration file name] open error - [system generated letter file read error message] |
|            |                                                 | [configuration file name] read error - [system generated letter file read error message] |
| 502        | Config file validation error                    | Duplicate Param file entry([parameter name])                                             |
|            |                                                 | Invalid Param Value([parameter name])                                                    |
|            |                                                 | No loan types found in CFG Letter file                                                   |
|            |                                                 | Invalid loan tracking type([4-digit tracking type)                                       |
|            |                                                 | Invalid sub source code([3-digit sub source code])                                       |
|            |                                                 | Invalid other action([3-digit other action])                                             |
|            |                                                 | Test mode on but no test account(s) defined                                              |
|            |                                                 | Invalid start/end date(s)                                                                |
| 503        | Invalid loan id                                 | [loan id]                                                                                |
| 504        | Attempt to skip a payment on an ineligible loan | [loan id]                                                                                |
| 505        | Invalid fee share id                            | [share id]                                                                               |
| 506        | Loan Record Update Failed                       | [file maint system error]                                                                |
| 507        | Tracking Record Create Failed                   | [file maint system error]                                                                |
| 508        | Skip Pay Fee Post Failed                        | [tran posting system error]                                                              |
