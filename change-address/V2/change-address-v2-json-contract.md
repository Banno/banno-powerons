# Change Address V2 JSON contract

## PROCESSADDRCHANGE state

### UX Request (PROCESSADDRCHANGE)

```json
{
  "rgState": "PROCESSADDRCHANGE",
  "powerOnFilename": "BANNO.CHANGE.ADDR.V2.POW",
  "userCharList": [
    { "id": 1, "value": "0123456789" }, //street address|extra address
    { "id": 2, "value": "0123456789" }, //city|state|zipcode
    { "id": 3, "value": "0123456789" }, //member note
    { "id": 4, "value": "0123456789" } //member's banno id
  ],
  "userNumList": [
    { "id": 1, "value": 124521452 } //locator of targetted name record
  ],
  "rgSession": 1
}
```

**Request Detail:**

- userChrList[1]: street address|extra address
- userChrList[2]: city|state|zipcode
- userChrList[3]: member note optionally entered by member in ux
- userChrList[4]: member's banno id guid
- userNumList[1]: locator of targetted name record

### PowerOn Response (PROCESSADDRCHANGE)

```json
{
  "responseCode": "504",
  "loggingErrorMessage": "",
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.CHANGE.ADDR.V2.POW",
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
    }
  }
}
```

**Response detail:**

- responseCode: operation response code
- loggingErrorMessage: logging error code/text associated with the responseCode
- Results: results object
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

## Response Codes

See the Modifier section for additional details. The Modifier is appended to the main Logging Error Message.

Below is the inclusive list of Response Codes that may be returned by the PowerOn:

| Response Code | Logging Error Message              | Modifier                                                                                 |
| ------------- | ---------------------------------- | ---------------------------------------------------------------------------------------- |
| 500           | Program running in memo mode       |                                                                                          |
| 501           | Error opening/reading config file  | [configuration file name] open error - [system generated letter file read error message] |
|               |                                    | [configuration file name] read error - [system generated letter file read error message] |
| 502           | Config file validation error       | Duplicate Param file entry([parameter name])                                             |
|               |                                    | Invalid Acct Types Setup                                                                 |
|               |                                    | Invalid Warning Types Setup                                                              |
|               |                                    | Invalid Match Name Types Setup                                                           |
|               |                                    | Invalid Clear Warning Type Setup                                                         |
|               |                                    | Invalid Set Warning Type Setup                                                           |
|               |                                    | PO Box Types Setup Incomplete                                                            |
|               |                                    | No name types set for name level matching                                                |
|               |                                    | No email detail lines specified for member email                                         |
|               |                                    | Invalid from email address specified for member email                                    |
|               |                                    | Invalid email address specified for Conversations override                               |
| 503           | Ineligible Account                 | Acct Warning [4-digit account warning]                                                   |
|               |                                    | Acct Type [4-digit account type]                                                         |
| 504           | Successful Address Update          | [# of name record updates] Name Record                                                   |
|               |                                    | Name LOC [name record locator]                                                           |
| 505           | Error validating requested changes | Acct Warning [4-digit account warning]                                                   |
|               |                                    | No allowed name records found                                                            |
|               |                                    | Tracking 8 not found                                                                     |
|               |                                    | Tracking 8 without MbrAddr Link                                                          |
|               |                                    | Address entry exceeds Episys limits                                                      |
|               |                                    | Missing data in required field                                                           |
|               |                                    | Attempt to enter PO Box as street address                                                |
|               |                                    | Targeted name loc [targeted locator] not found                                           |
| 506           | Error processing requested changes | Could not locate name LOC [name locator]                                                 |
|               |                                    | Name FM - [system generated fm error]                                                    |
|               |                                    | [match level] LOC [name locator] FM - [system generated fm error]                        |
