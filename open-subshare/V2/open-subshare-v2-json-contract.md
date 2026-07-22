# Open Subshare v2 JSON contract

## PRELOADDATA state

### UX Request (PRELOADDATA)

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFilename": "BANNO.NEWSUBCREATE.V2.POW",
  "userCharList": [
    { "id": 1, "value": "0123456789" } //10-digit member number
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1]: Member number associated with the selected account (10 digits)

### PowerOn Response (PRELOADDATA)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.NEWSUBCREATE.V2.POW",
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
    "introText": [
      "Information message for the template line 1",
      "Information message for the template line 2"
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
- introText: Text that will display at the top of the initial page

## PROCESSDATA state

### UX Request (PROCESSDATA)

```json
{
  "rgState": "PROCESSDATA",
  "powerOnFilename": "BANNO.NEWSUBCREATE.V2.POW",
  "userCharList": [
    { "id": 1, "value": "0123456789" } //10-digit member number
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1]: Member number associated with the selected account (10 digits)

### PowerOn Response (PROCESSDATA)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.NEWSUBCREATE.V2.POW",
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
    "success": true
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
- success: boolean - true/false. Was the operation successful?

## Errors

Any and all errors should be conveyed via the following structure:

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.NEWSUBCREATE.V2.POW",
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

| Error Code | Logging Error Message                        | Modifier                                                                                 |
| ---------- | -------------------------------------------- | ---------------------------------------------------------------------------------------- |
| 500        | Program running in memo mode                 |                                                                                          |
| 501        | Config file open/read error                  | [configuration file name] open error - [system generated letter file read error message] |
|            |                                              | [configuration file name] read error - [system generated letter file read error message] |
| 502        | Account warning exists                       | warning code [3-digit warning code]                                                      |
| 503        | Ineligible account type                      | Acct type [4-digit account type]                                                         |
| 504        | No eligible shares to be created             |                                                                                          |
| 505        | Error reading ToC for group                  | Share Grp [2-digit group/category] [system generated letter file read error message]     |
| 506        | No Shares with sufficient funds to xfer      |                                                                                          |
| 507        | Error reading passed name info               |                                                                                          |
| 508        | Next ID calculation error                    |                                                                                          |
| 509        | Error getting new share rate                 |                                                                                          |
| 510        | Could not calculate new maturity date        |                                                                                          |
| 511        | Error creating new share                     | [share create error detail message]                                                      |
| 512        | Maximum share limit error                    |                                                                                          |
| 513        | Error calculating fee                        |                                                                                          |
| 514        | Error reading fee disclosure for group       | Share Grp [2-digit group/category] [system generated letter file read error message]     |
| 515        | No existing open Shares/Loans on the account |                                                                                          |
| 516        | Config file validation error                 | Duplicate Param file entry([parameter name])                                             |
|            |                                              | Invalid Param Value([parameter name])                                                    |
