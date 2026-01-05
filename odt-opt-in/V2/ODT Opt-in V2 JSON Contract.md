# ODT Opt-in V2 JSON Contract

## PRELOADDATA state

### UX Request (PRELOADDATA)

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFilename": "BANNO.ODTOPTIN.V2.POW",
  "userCharList": [],
  "userNumList": [
    { "id": 1, "value": 100 } //max number of shares to process **optional**
  ],
  "rgSession": 1
}
```

**Request Detail:**

- userNumList[1]: Optionally pass the max number of shares that should be processed.

### PowerOn Response (PRELOADDATA)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.ODTOPTIN.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/04/26 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "01/01/2026",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "maxSharesExceeded": false,
    "shareDetail": [
      {
        "SID": "0000",
        "name": "My Primary Share",
        "balance": "######9.99",
        "currentState": false
      },
      {
        "SID": "0010",
        "name": "SuperDuper Checking",
        "balance": "######9.99",
        "currentState": true
      }
    ],
    "terms": [
      "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
    ],
    "feeDisclosure": [
      "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
    ],
    "revocationInstructions": [
      "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
    ],
    "servicesInstructions": [
      "Check the box for an account to opt-in to overdraft services.  Un-check the box for an",
      "account to opt-out of overdraft services."
    ],
    "optInInformationText": [
      "This account will be opted-in to overdraft services. The fee disclosure below applies."
    ],
    "optOutInformationText": [
      "This account will be opted-out from overdraft services."
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
- maxSharesExceeded: boolean - true/false. Did the program find more than 13 eligible shares for this member?
- shareDetail: array of share details objects
  - SID: Share ID [SHARE:ID]
  - name: Share description [SHARE:DESCRIPTION]
  - balance: Share available balance [SHARE:AVAILABLEBALANCE]
  - currentState: boolean - true/false. Is the share enrolled in ODT services
- terms: Custom Terms
- feeDisclosure: Custom fee disclosure
- revocationInstructions: Custom revocation instructions
- servicesInstuctions: Custom services instructions
- optInInformationText: Custom opt-in information text
- optOutInformationText: Custom opt-out information text
- optInOutInfoText: Custom opt-in/out information test **optional**

## PROCESSDATA state

### UX Request (PROCESSDATA)

UX returns updated state of each share. Share IDs listed are to be enrolled into ODT services. Valid share IDs not explicitly listed are not to be enrolled into ODT services.

```json
{
  "rgState": "PROCESSDATA",
  "powerOnFilename": "BANNO.ODTOPTIN.V2.POW",
  "userChrList": [
    { "id": 1, "value": "0000,0010,0012,0014" },
    { "id": 2, "value": "" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
    { "id": 1, "value": 100 } //max number of shares to process **optional**
  ],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1-5]: Comma delimited list of 4-digit share IDs to be enrolled into ODT services. Each userCharList array slot can contain up to 26, 4-character share IDs for a maximum of 130.

### PowerOn Response (PROCESSDATA)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.ODTOPTIN.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/04/26 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "01/01/2026",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "maxSharesExceeded": false, //'true' if the number of shares found exceeds processing capabilities (130 shares)
    "successMessage": ["an", "array", "of", "lines"],
    "shareDetailUpdated": [
      {
        "SID": "0000",
        "name": "My Primary Share",
        "balance": "######9.99",
        "currentState": false
      },
      {
        "SID": "0010",
        "name": "SuperDuper Checking",
        "balance": "######9.99",
        "currentState": true
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
- maxSharesExceeded: boolean - true/false. Did the program find more than 13 eligible shares for this member?
- successMessage: CU customizable success message.
- shareDetailUpdated: array of updated share detail objects
  - SID: Share ID [SHARE:ID]
  - name: Share description [SHARE:DESCRIPTION]
  - balance: Share available balance [SHARE:AVAILABLEBALANCE]
  - currentState: boolean - true/false. Is the share enrolled in ODT services? The updated state.

## Errors

### General Error

Any and all errors should be conveyed via the following structure:

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.ODTOPTIN.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/04/26 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "01/01/2026",
        "slidLength": 4,
        "memoMode": false
      }
    },
    "errorCode": 505,
    "errorMessage": "if error processing, something to log",
    "errorDisplayMessage": ["Optional member display message - see below"]
  }
}
```

- errorCode: Error code generated (numeric)
- errorMessage: Error message - text description of the errorCode
- errorDisplayMessage: **\*\*Optional** An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message. **Must be supported by UX code**

### Memo Mode Error

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.ODTOPTIN.V2.POW",
        "version": "2.0.0",
        "lastModDate": "01/04/26 16:00 MT",
        "language": 1,
        "note1": "New PowerOn"
      },
      "systemInfo": {
        "systemDate": "01/01/2026",
        "slidLength": 4,
        "memoMode": true
      }
    },
    "errorCode": 500,
    "errorMessage": "Program running in memo mode",
    "memoModeMessage": [
      "Nightly processing is underway. We are unable to complete",
      "your overdraft settings change request."
    ]
  }
}
```

## Error Codes

See the Modifier section for additional details. The Modifier is appended to the main Logging Error Message.

Below is the inclusive list of Error Codes that may be returned by the PowerOn:

| Error Code | Logging Error Message                           | Modifier                                                                                 | Additional Notes                  |
| ---------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------- | --------------------------------- |
| 500        | Program running in memo mode                    |                                                                                          |                                   |
| 501        | Error opening/reading config file               | [configuration file name] open error - [system generated letter file read error message] |                                   |
|            |                                                 | [configuration file name] read error - [system generated letter file read error message] |                                   |
| 502        | Config file validation error                    | Duplicate Param file entry([parameter name])                                             |                                   |
|            |                                                 | Invalid Param Value([parameter name])                                                    |                                   |
|            |                                                 | Invalid tracking type                                                                    |                                   |
|            |                                                 | Invalid source code for SCT                                                              | See CFG comments for valid values |
|            |                                                 | Invalid source code for SCF                                                              | See CFG comments for valid values |
|            |                                                 | Invalid auth/fee option for AFT                                                          | See CFG comments for valid values |
|            |                                                 | Invalid auth/fee option for AFF                                                          | See CFG comments for valid values |
| 503        | No eligible shares                              |                                                                                          |                                   |
| 504        | Ineligible Acct Type found                      | Type [invalid account type]                                                              |                                   |
| 505        | Account warning exists                          | Warnings found: [list of account warnings found]                                         |                                   |
| 506        | Error updating share tracking                   |                                                                                          |                                   |
| 507        | Error updating source code & auth/fee fields    |                                                                                          |                                   |
| 508        | Error updating share overdraft tolerance amount |                                                                                          |                                   |
| 509        | n/a                                             |                                                                                          | Used by custom version of PowerOn |
| 510        | Multiple update errors                          |                                                                                          |                                   |
