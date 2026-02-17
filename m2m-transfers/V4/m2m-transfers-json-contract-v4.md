# Member to Member Transfer V4 JSON contract

## PRELOADDATA state

### UX Request (PRELOADDATA)

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFilename": "BANNO.M2MTRANSFERS.V4.POW",
  "userChrList": [], // userchar[1-5] is unused
  "userNumList": [], // usernum[1-5] is unused
  "rgSession": 1
}
```

**Request Detail:**

- userCharList[1-5]: unused
- userCharList[1-5]: unused

### PowerOn Response (PRELOADDATA)

#### Success (PRELOADDATA)

list of institution and member limits, eligible shares, list of scheduled transfers, list of saved recipients

```json
{
  "generalSpecifications": {
    "programInfo": {
      "name": "BANNO.M2MTRANSFERS.V4.POW",
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
  "currentState": {
    "slidLength": 4,
    "systemDate": "11/30/2021",
    "transferLimits": {
      "enforceLimits": true, // remaining transferLimit properties are optional, if enforceLimits is false
      "countLimit": "5", // if limits are 0 assume no limit
      "memberCount": "1",
      "amountLimit": "1000.00",
      "memberAmount": "100.00",
      "perTransferLimit": "500.00"
    },
    "availableShares": [
      {
        "transferSLId": "0000800005S0002",
        "type": "savings",
        "name": "SUMMER SAVER",
        "available": "50.00",
        "accountOwnerName": "Beth Nussbaum",
        "crossAccount": true
      },
      {
        "transferSLId": "0000800005S0010",
        "type": "checking",
        "name": "REGULAR CHECKING",
        "available": "500.00",
        "accountOwnerName": "Ruth Nordstrom",
        "crossAccount": false
      },
      {
        "transferSLId": "0000800005S0015",
        "type": "checking",
        "name": "ULTIMATE CHECKING (10)",
        "available": "0.00",
        "accountOwnerName": "Cliff Woodward",
        "crossAccount": true
      }
    ],
    "scheduledTransfers": [
      {
        "transferLoc": "215",
        "transferCreateDate": "08/31/2021",
        "sourceAccount": "1234567890S0020",
        "accountName": "Bob's CHECKING",
        "transferAmt": "120.00",
        "recipientName": "FIT", // first 3 letters of last name or business name
        "recipientMemberId": "9876543210",
        "recipientAccountType": "savings", // savings, checking or loan
        "recipientAccountId": "0001", // optional share or loan id
        "recipientNickname": "Emmy", // optonal, blank if not saved
        "startDate": "07/31/2021",
        "nextTransferDate": "08/07/2021",
        "transferFrequency": "weekly",
        "day1": "", // semi-monthly - first day
        "day2": "", // semi-monthly - second day
        "readOnly": false
      },
      {
        "transferLoc": "395",
        "transferCreateDate": "08/31/2021",
        "sourceAccount": "1234567890S0001",
        "accountName": "MY Savings",
        "transferAmt": "120.00",
        "recipientName": "CLE",
        "recipientMemberId": "9876543210",
        "recipientAccountType": "loan",
        "recipientAccountId": "0001",
        "recipientNickname": "",
        "startDate": "07/03/2021",
        "nextTransferDate": "08/07/2021",
        "transferFrequency": "semi-monthly",
        "day1": "3",
        "day2": "31",
        "readOnly": true
      }
    ],
    "savedRecipients": [
      {
        "recipientLoc": "5443231543",
        "recipientName": "FLO",
        "recipientMemberId": "9876543210",
        "recipientAccountType": "savings",
        "recipientAccountId": "0001", // optional
        "recipientNickname": "Zeke's Future"
      },
      {
        "recipientLoc": "5431543",
        "recipientName": "CRE",
        "recipientMemberId": "9876543210",
        "recipientAccountType": "loan",
        "recipientAccountId": "", // optional
        "recipientNickname": "Sally Martin"
      }
    ],
    "labels": {
      "memberNameSubTitle": null,
      "idSubTitle": null
    },
    "shareAcctIdOptional": false
  }
}
```

**Response detail:**

- generalSpecifications: Program and system info
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
- currentState: currentState object

#### Errors (PRELOADDATA)

Configuration File read error

```jsonc
{
  "generalSpecifications": {
    "programInfo": {
      // object details excluded -- see details above
    },
    "systemInfo": {
      // object details excluded -- see details above
    },
  },
  "errorCode": "501",
  "loggingErrorMessage": "Config file error: BANNO.M2MTRANSFERS.V4.CFG open error - No such file or directory",
}
```

Invalid Account (by Account Warning)

```jsonc
{
  "generalSpecifications": {
    "programInfo": {
      // object details excluded -- see details above
    },
    "systemInfo": {
      // object details excluded -- see details above
    },
  },
  "errorCode": "502",
  "loggingErrorMessage": "Invalid Account - account warning xxx",
}
```

No Eligible Share(s) found

```jsonc
{
  "generalSpecifications": {
    "programInfo": {
      // object details excluded -- see details above
    },
    "systemInfo": {
      // object details excluded -- see details above
    },
  },
  "errorCode": "503",
  "loggingErrorMessage": "No eligible shares found",
}
```

## VERIFYMEMBER state

### UX Request (VERIFYMEMBER)

```jsonc
{
  "rgState": "VERIFYMEMBER",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V4.POW",
  "userChrList": [
    { "id": 1, "value": "9876543210|HUB|accountType|0234" }, // [member id]|[first 3 of last name or business name]|[accountType]|[accountId]
  ],
  "userNumList": [],
  "rgSession": 1,
}
```

**Request Detail:**

- userCharList[1]: Member verification information

### PowerOn Response (VERIFYMEMBER)

#### Success (VERIFYMEMBER)

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        // object details excluded -- see details above
      },
      "systemInfo": {
        // object details excluded -- see details above
      }
    },
    "verified": true,
    "recipientAccountId": "0010" // account ID
  }
}
```

**Response detail:**

- results: results object
  - generalSpecifications: **See above**
  - verified: boolean - true/false. Was the request verified?
  - recipientAccountId: Verified account Id

#### Errors (VERIFYMEMBER)

```jsonc
{
  "generalSpecifications": {
    "programInfo": {
      // object details excluded -- see details above
    },
    "systemInfo": {
      // object details excluded -- see details above
    },
  },
  "errorCode": "509",
  "loggingErrorMessage": "Member verification failed: Name verification failed",
}
```

## CREATETRAN state

Create new transfer with new or existing recipient

- Existing Recipient: recipientLoc will be present
- New Recipient: if nickname is present, create new
- All recipients: sourceAccount, destinationAccount, transferAmt, transferFrequency, startDate, day1, day2
- One-time immediate transfers have an optional internal memo field.

### UX Request (CREATETRAN)

```jsonc
{
  "rgState": "CREATETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V4.POW",
  "userChrList": [
    {
      "id": 1,
      "value": "1234567890S0001|9876543210|L0001|weekly|12/31/2021|1|15",
    }, // [sourceAccount][recipient member id][recipient S/L id][frequency]|[startDate or "soonest" ]|[day1]|[day2]
    { "id": 2, "value": "HUB|nickname" }, // [first 3]|[nickname]
    { "id": 3, "value": "internal memo for immediate transfers" }, // internal memo for immediate transfers (max 132 characters)
    { "id": 4, "value": "1000.51" }, // transfer amount
  ],
  "userNumList": [
    { "id": 1, "value": 395 }, // recipientLoc if available
  ],
  "rgSession": 1,
}
```

**Request Detail:**

- userCharList[1]: Transfer details
- userCharList[2]: First name/nickname
- userCharList[3]: Transfer memo
- userCharList[4]: Transfer amount
- userNumList[1]: Recipient locator

### PowerOn Response (CREATETRAN)

```jsonc
{
  "history": {
    // used for history event by node-poweron-proxy. stripped before being sent to client
    "change": {
      "application": "member-to-member-transfers-poweron",
      "name": "MemberToMemberTransferScheduled",
      "toMemberName": "John Smith",
      "amount": 1.0,
      "toAccountName": "Regular Shares",
      "fromAccountNumberMasked": "x00S0010",
      "toAccountNumberMasked": "x55S0001",
      "nextTransferDate": "08/07/2021",
      "frequency": "weekly", // included if frequency is not "once"
      "day1": "", // included if frequency is not "once"
      "day2": "", // included if frequency is not "once"
    },
  },
  "results": {
    "generalSpecifications": {
      "programInfo": {
        // object details excluded -- see details above
      },
      "systemInfo": {
        // object details excluded -- see details above
      },
    },
    "success": true,
    "memoMode": false,
    "memoModeSuccessMessage": ["memo mode success line 1","memo mode success line 2"],
    "transferLoc": "395",
    "recipientLoc": "54315431",
    "currentState": "[PRELOADDATA]",
  },
}
```

**Response detail:**

- history: history object
- results: results object
  - generalSpecifications: **See above**
  - success: boolean - true/false. Was transaction successful?
  - memoMode: boolean - true/false. Is the system in memo mode?
  - memoModeSuccessMessage: Message that will display if transfer completed during memo mode
  - transferLoc: Transfer locator
  - recipientLoc: Recipient locator
  - currentState: **See currentState definition in PRELOADDATA response**

## EDITTRAN state

Edit existing transaction (expire existing transfer & create a new transfer) \* transferLoc, sourceAccount, transferAmt, transferFrequency, endDate, day1, day2

### UX Request (EDITTRAN)

```jsonc
{
  "rgState": "EDITTRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V4.POW",
  "userChrList": [
    { "id": 1, "value": "weekly|12/31/2021|1|15" }, // [transferFrequency]|[startDate]|[day1]|[day2] max 132 characters
    { "id": 2, "value": "internal memo for immediate transfers" }, // internal memo for immediate transfers only
    { "id": 3, "value": "1000.51" }, // transfer amount
  ],
  "userNumList": [
    { "id": 1, "value": 395 }, // transferLoc
  ],
  "rgSession": 1,
}
```

**Request Detail:**

- userCharList[1]: Transfer edit details
- userCharList[2]: Transfer memo/immediate transfers
- userCharList[3]: Transfer amount
- userNumList[1]: Transfer locator

### PowerOn Response (EDITTRAN)

```jsonc
{
  "history": {
    // used for history event by node-poweron-proxy. stripped before being sent to client
    "change": {
      "application": "member-to-member-transfers-poweron",
      "name": "MemberToMemberTransferUpdated",
      "toMemberName": "John Smith",
      "amount": 1.0,
      "toAccountName": "Regular Shares",
      "fromAccountNumberMasked": "x00S0010",
      "toAccountNumberMasked": "x55S0001",
      "nextTransferDate": "08/07/2021",
      "frequency": "weekly", // included if frequency is not "once"
      "day1": "", // include dif frequency is not "once"
      "day2": "", // include dif frequency is not "once"
    },
  },
  "results": {
    "generalSpecifications": {
      "programInfo": {
        // object details excluded -- see details above
      },
      "systemInfo": {
        // object details excluded -- see details above
      },
    },
    "success": true,
    "memoMode": false,
    "memoModeSuccessMessage": ["memo mode success line 1","memo mode success line 2"],
    "transferLoc": "395",
    "currentState": "[PRELOADDATA]",
  },
}
```

**Response detail:**

- history: history object
- results: results object
  - generalSpecifications: **See above**
  - success: boolean - true/false. Was transaction edit successful?
  - memoMode: boolean - true/false. Is the system in memo mode?
  - memoModeSuccessMessage: Message that will display if transfer completed during memo mode
  - transferLoc: Transfer locator
  - currentState: **See currentState definition in PRELOADDATA response**

## DELETERECIP state

### UX Request (DELETERECIP)

```jsonc
{
  "rgState": "DELETERECIP",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V4.POW",
  "userChrList": [],
  "userNumList": [
    { "id": 1, "value": 395 }, // recipientLoc
  ],
  "rgSession": 1,
}
```

**Request Detail:**

- userNumList[1]: Recipient locator

### PowerOn Response (DELETERECIP)

```jsonc
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        // object details excluded -- see details above
      },
      "systemInfo": {
        // object details excluded -- see details above
      },
    },
    "success": true,
    "recipientLoc": 395,
    "currentState": "[PRELOADDATA]",
  },
}
```

**Response detail:**

- results: results object
  - generalSpecifications: **See above**
  - success: boolean - true/false. Was delete recipient successful?
  - recipientLoc: Recipient locator
  - currentState: **See currentState definition in PRELOADDATA response**

## DELETETRAN state

### UX Request (DELETETRAN)

```jsonc
{
  "rgState": "DELETETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V4.POW",
  "userChrList": [],
  "userNumList": [
    { "id": 1, "value": 395 }, // transferLoc
  ],
  "rgSession": 1,
}
```

**Request Detail:**

- userNumList[1]: Transfer locator

### PowerOn Response (DELETETRAN)

```jsonc
{
  "history": {
    // used for history event by node-poweron-proxy. stripped before being sent to client
    "change": {
      "application": "member-to-member-transfers-poweron",
      "name": "MemberToMemberTransferDeleted",
      "toMemberName": "John Smith",
      "amount": 1.0,
      "toAccountName": "Regular Shares",
      "fromAccountNumberMasked": "x00S0010",
      "toAccountNumberMasked": "x55S0001",
      "nextTransferDate": "08/07/2021",
      "frequency": "weekly", // included if frequency is not "once"
      "day1": "", // include dif frequency is not "once"
      "day2": "", // include dif frequency is not "once"
    },
  },
  "results": {
    "generalSpecifications": {
      "programInfo": {
        // object details excluded -- see details above
      },
      "systemInfo": {
        // object details excluded -- see details above
      },
    },
    "success": true,
    "memoMode": false,
    "transferLoc": 395,
    "currentState": "[PRELOADDATA]",
  },
}
```

**Response detail:**

- history: history object
- results: results object
  - generalSpecifications: **See above**
  - success: boolean - true/false. Was delete transfer successful?
  - memoMode: boolean - true/false. Is the system in memo mode?
  - transferLoc: Transfer locator
  - currentState: **See currentState definition in PRELOADDATA response**

## Errors

All errors should be conveyed via one of the following structures.

### Error - `results` Object

```json
{
  "results": {
    "generalSpecifications": {
      "programInfo": {
        "name": "BANNO.M2MTRANSFERS.V4.POW",
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

### Error - No `results` Object

```json
{
  "generalSpecifications": {
    "programInfo": {
      "name": "BANNO.M2MTRANSFERS.V4.POW",
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
```

- errorCode: Error code generated (numeric)
- errorMessage: Error message - text description of the errorCode
- errorDisplayMessage: **\*\*Optional** An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message. **Must be supported by UX code**

## Error Codes

| Error Code | Logging Error Message             | Modifier                                                                                 | Additional Notes As Needed                                     |
| ---------- | --------------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| 500        | Program running in memo mode      |                                                                                          |                                                                |
| 501        | Config file error                 | [configuration file name] open error - [system generated letter file read error message] |                                                                |
|            |                                   | [configuration file name] read error - [system generated letter file read error message] |                                                                |
|            |                                   | Invalid Parameter in CFG file                                                            |                                                                |
| 502        | Invalid Source Account            | Pref Access type 3 not found                                                             |                                                                |
|            |                                   | Acct Warning 1234                                                                        |                                                                |
|            |                                   | No eligible transfer from shares/loans                                                   |                                                                |
|            |                                   | Acct Type 1234                                                                           |                                                                |
| 503        | Invalid Recipient Account         |                                                                                          |                                                                |
| 504        | Insufficient Information          | Cannot calc member limits.                                                               |                                                                |
| 505        | Invalid Input                     |                                                                                          |                                                                |
| 506        | Error Processing Recipient Record |                                                                                          | Error creating/deleting recipient                              |
| 507        | Error Processing Transfer Record  | No Modifier. Error Code 507 does not always provide a Modifier.                          | This set of errors is for creating/updating/deleting transfers |
|            |                                   | Target s/l xfer Loc 1234567 not found                                                    |                                                                |
|            |                                   | Target ID not found or invalid                                                           |                                                                |
| 508        | Undefined Error                   |                                                                                          |                                                                |
| 509        | Member verification failed        | Member account not found                                                                 | This set of errors is for member unverified                    |
|            |                                   | Account Closed - 99/99/99                                                                |                                                                |
|            |                                   | Name verification failed                                                                 |                                                                |
|            |                                   | Must be a different member account number                                                |                                                                |
|            |                                   | S/L closed or charged-off - 99/99/99                                                     |                                                                |
|            |                                   | S/L ID not found                                                                         |                                                                |
|            |                                   | No valid share or loan found                                                             |                                                                |
|            |                                   | S/L missing service code                                                                 |                                                                |
|            |                                   | Invalid S/L type                                                                         |                                                                |
|            |                                   | Invalid share code                                                                       |                                                                |
|            |                                   | Loan has $0.00 payoff                                                                    |                                                                |
|            |                                   | Invalid IRS code                                                                         |                                                                |
| 510        | Account ID incorrect              |                                                                                          |                                                                |
| 511        | Request exceeds limits            |                                                                                          |                                                                |
| 512        | Config file validation error      | Duplicate Param file entry([parameter name])                                             |                                                                |
|            |                                   | Invalid Param Value([parameter name])                                                    |                                                                |

## Transfer Frequencies

_The following strings are all valid transfer frequencies:_

- `once`
- `weekly`
- `monthly`
- `semiMonthly`
- `biweekly`
- `quarterly`
- `yearly`

## Account types

The recipient member's account id is optional when creating a new transfer and when displaying saved recipients

_Available account types are:_

- `savings`
- `checking`
- `loan`
