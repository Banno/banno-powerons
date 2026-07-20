# Withdraw by Check V2 JSON contract

## STATESTART state

### UX Request (STATESTART)

```json
{
  "rgState": "STATESTART",
  "powerOnFileName": "BANNO.CHECK.WITHDRAW.V2.POW",
  "userChrList": [{ "id": 1, "value": "0123456789S0123" }],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userChrList[1]: Account and share or loan ID (13 or 15 character string)
  - 10-digit member number
  - 'S' for Share or 'L' for Loan
  - 2 or 4 digit Share or Loan ID
- userChrList[2-5]: unused
- userNumList[1-5]: unused

### PowerOn Response (STATESTART)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.CHECK.WITHDRAW.V2.POW",
        "version": "0.1.0",
        "lastModDate": "01/31/24 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2024",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "eligible": true,
    "memberAccountNumber": "0123456789S0123",
    "shareLoanDescription": "ADVANCED CHECKING",
    "available": "123456.00",
    "minWdAmount": "123456.00",
    "maxWdAmount": "123456.00",
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
- eligible: Is the member eligible for this service (boolean).
- memberAccountNumber: The 10-digit account number, 'S' or 'L' for Share or Loan, Share or Loan ID
- shareLoanDescription: Share or Loan nickname if available else Share or Loan description
- available:
  - For shares, the available balance
  - For loans, if loan code=3 and loan interest type=10-1899 then loan available cash advance else loan available credit
- minWdAmount: the minimum WD amount (from parameter settings)
- maxWdAmount: the maximum WD amount (from parameter settings)
- owner: The primary member's long name (NAME:LONGNAME)
- address: The system calculated mailing address (ACCOUNT:PAYEELINE[1-6])
- disclaimerText: Custom terms and conditions as set up in the parameter settings Letter file.

## PERFORMWITHDRAW state

### UX Request (PERFORMWITHDRAW)

```json
{
  "rgState": "PERFORMWITHDRAW",
  "powerOnFileName": "BANNO.WITHDRAW.CHECK.V2.POW",
  "userChrList": [
    { "id": 1, "value": "0123456789S0123" },
    { "id": 2, "value": "2.00" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userChrList[1]: Account and share or loan ID (13 or 15 character string)
  - 10-digit member number
  - 'S' for Share or 'L' for Loan
  - 2 or 4 digit Share or Loan ID
- userChrList[2]: Dollar amount of withdrawal
- userChrList[3-5]: unused
- userNumList[1-5]: unused

### PowerOn Response (PERFORMWITHDRAW)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.CHECK.WITHDRAW.V2.POW",
        "version": "0.1.0",
        "lastModDate": "01/31/24 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "02/01/2024",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "success": true,
    "memoMode": false
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
- success: Was the operation successful? (boolean) - true/false
- memoMode: Is the system in MemoMode? (boolean) - true/false

## Errors

Any and all errors should be conveyed via the following structure:

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.CHECK.WITHDRAW.V2.POW",
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
    "errorDisplayMessage": ["Optional member display message - see below"],
    "requested": "123456.00",
    "minWdAmount": "123456.00",
    "maxWdAmount": "123456.00"
  }
}
```

- errorCode: Error code generated (numeric)
- errorMessage: Error message - text description of the errorCode
- errorDisplayMessage: **\*\*Optional** An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message. **Must be supported by UX code**
- requested: The amount of the WD that was requested _--for Error Code 508 only-_
- minWdAmount: the minimum WD amount (from parameter settings) _--for Error Code 508 only--_
- maxWdAmount: the maximum WD amount (from parameter settings) _--for Error Code 508 only--_

Possible error codes include:

## Error Codes

| Error Code | Logging Error Message          | Modifier                                                                                 | Additional Notes As Needed                              |
| ---------- | ------------------------------ | ---------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| 500        | System is in memo mode         |                                                                                          |                                                         |
| 501        | Config file open/read error    | [configuration file name] open error - [system generated letter file read error message] |                                                         |
|            |                                | [configuration file name] read error - [system generated letter file read error message] |                                                         |
| 502        | Config file validation error   | Duplicate Param file entry([parameter name])                                             |                                                         |
|            |                                | Invalid Param Value([parameter name])                                                    |                                                         |
| 503        | Avail. Balance <= $0.00        |                                                                                          | Target Share/Loan available balance<=$0.00              |
| 504        | Invalid Address                |                                                                                          |                                                         |
| 505        | Not Found                      | Account Not Found                                                                        | Error Code 505 is used for account find/validate errors |
|            |                                | Account Warning Found: [3-digit comma separated account warning list]                    |                                                         |
|            |                                | Share Not Found                                                                          |                                                         |
|            |                                | Loan Not Found                                                                           |                                                         |
|            |                                | Invalid Share Type: [4-digit share type]                                                 |                                                         |
|            |                                | Invalid Loan Type: [4-digit loan type]                                                   |                                                         |
|            |                                | Share Warning Found: [3-digit comma separated share warning list]                        |                                                         |
|            |                                | Loan Warning Found: [3-digit comma separated loan warning list]                          |                                                         |
| 506        | Reg D Limit                    | [transaction posting system error message]                                               | Reg D Limit                                             |
| 507        | Cross Account WD Attempted     |                                                                                          |                                                         |
| 508        | Amount requested out of bounds |                                                                                          |                                                         |
| 509        | Insufficient Funds             | Amount req. ###,##9.99 exceeds avail. ###,##9.99                                         |                                                         |
|            |                                | [transaction posting system error message]                                               |                                                         |
| 510        | Unhandled Error                |                                                                                          | (catch-all)                                             |
| 511        | TRANPERFORM Other Error        | [transaction posting system error message]                                               | other transaction posting system error                  |
