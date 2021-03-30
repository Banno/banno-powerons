#JSON contract for BANNO.M2MTRANSFERS.V1.POW

## GET PRELOADDATA STATE
### Client Request (PRELOADDATA)

```jsonc
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [], // userchar[1-5] is unused
  "userNumList": [], //  usernum[1-5] is unused
  "rgSession": 1
}
```

### PowerOn Successful Response (PRELOADDATA)
list of institution and member limits, eligible shares, list of scheduled transfers, list of saved recipients

```jsonc
{
  "currentState": {
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
        "name": "SUMMER SAVER",
        "available": "50.00",
        "accountOwnerName": "Beth Nussbaum"
      },
      {
        "transferSLId": "0000800005S0010",
        "name": "REGULAR CHECKING",
        "available": "500.00",
        "accountOwnerName": "Ruth Nordstrom"
      },
      {
        "transferSLId": "0000800005S0015",
        "name": "ULTIMATE CHECKING (10)",
        "available": "0.00",
        "accountOwnerName": "Cliff Woodward"
      }
    ],
    "scheduledTransfers": [
      {
        "transferLoc": "215",
        "sourceAccount": "1234567890S0020",
        "accountName": "Bob's CHECKING",
        "transferAmt": "120.00",
        "recipientName": "FIT", // first 3 letters of last name or business name
        "recipientMemberId": "9876543210",
        "recipientAccountType": "savings", // savings, checking or loan
        "recipientAccountId": "0001", // optional share or loan id
        "recipientNickname": "Emmy", // optonal, blank if not saved
        "startDate": "07/31/2021",
	      "nextTransferDate":"08/07/2021",
        "transferFrequency": "weekly",
        "day1": "", // semi-monthly - first day
        "day2": "", // semi-monthly - second day
	      "readOnly": false
      },
      {
        "transferLoc": "395",
        "sourceAccount": "1234567890S0001",
        "accountName": "MY Savings",
        "transferAmt": "120.00",
        "recipientName": "CLE",
        "recipientMemberId": "9876543210",
        "recipientAccountType": "loan",
        "recipientAccountId": "0001",
        "recipientNickname": "",
        "startDate": "07/03/2021",
	      "nextTransferDate":"08/07/2021",
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
        "recipientAccountId": "0001",// optional
        "recipientNickname": "Zeke's Future"
      },
      {
        "recipientLoc": "5431543",
        "recipientName": "CRE",
        "recipientMemberId": "9876543210",
        "recipientAccountType": "loan",
        "recipientAccountId": "",// optional
        "recipientNickname": "Sally Martin"
      }
    ]
  }
}
```
### PowerOn Error Responses (PRELOADDATA)
#### PowerOn response - Configuration File read error:
```jsonc
{
  "errorCode": "501",
  "loggingErrorMessage": "Error [opening/reading] from config file: [error msg]"
}
```
### PowerOn response - Invalid Account (by Account Warning):

```jsonc
{
  "errorCode": "502",
  "loggingErrorMessage": "Invalid Account - account warning xxx"
}
```
### PowerOn response - No Eligible Share(s) found:

```jsonc
{
  "errorCode": "503",
  "loggingErrorMessage": "No eligible shares found"
}
```

## VERIFY MEMBER DETAILS
### Client Request (VERIFYMEMBER)

```jsonc
{
  "rgState": "VERIFYMEMBER",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "9876543210|HUB|accountType" }// [member id][first 3 of last name or business name][accountType]
  ],
  "userNumList": [],
  "rgSession": 1
}
```

### Poweron Successful Response (VERIFYMEMBER)

```jsonc
{
  "results": {
    "verified": true
  }
}
```

### Poweron Error Response (VERIFYMEMBER)

```jsonc
{
  "results": {
    "errorCode": "509",
    "loggingErrorMessage": "member verification failed"
  }
}
```

## CREATE TRANSFER REQUEST STATE
### Client Request (CREATETRAN)

```jsonc
{
  "rgState": "CREATETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890S0001|9876543210|L0001|weekly|12/31/2021|1|15" },  // [sourceAccount][recipient member id][account type or id][frequency]|[startDate]|[day1]|[day2]
    { "id": 2, "value": "HUB|nickname" }, // [first 3][nickname]
    { "id": 3, "value": "internal memo for immediate transfers" }, // internal memo for immediate transfers (max 132 characters)
    { "id": 4, "value":"1000.51"} // transfer amount
  ],
  "userNumList": [
    { "id": 1, "value": 395 } // recipientLoc if available
  ],
  "rgSession": 1
}
```
* CREATETRAN - Create new transfer with new or existing recipient
	* Existing Recipient:  recipientLoc will be present
	* New Recipient: if nickname is present, create new
	* All recipients: sourceAccount, destinationAccount, transferAmt, transferFrequency, startDate, day1, day2
	* One-time immediate transfers have an optional internal memo field.

### PowerOn Successful Response (CREATETRAN)
```jsonc
{
  "history": { // used for history event by node-poweron-proxy. stripped before being sent to client
   "change": {
    "application": "member-to-member-transfers-poweron",
    "name": "MemberToMemberTransferScheduled",
    "toMemberName": "John Smith",
    "amount": 1.00,
    "toAccountName": "Regular Shares",
    "fromAccountNumberMasked": "x00S0010",
    "toAccountNumberMasked": "x55S0001",
    "nextTransferDate": "08/07/2021",
    "frequency": "weekly", // included if frequency is not "once"
    "day1": "", // included if frequency is not "once"
    "day2": "" // included if frequency is not "once"
    }
  },
  "results": {
    "success": true,
    "memoMode": false,
    "transferLoc": "395",
    "recipientLoc": "54315431",
    "currentState": "[PRELOADDATA]"
  }
}
```

### Client Request (EDITTRAN)

```jsonc
{
  "rgState": "EDITTRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "weekly|12/31/2021|1|15" }, // [transferFrequency]|[startDate]|[day1]|[day2] max 132 characters
    { "id": 2, "value": "internal memo for immediate transfers" }, // internal memo for immediate transfers only
    { "id": 3, "value": "1000.51" } // transfer amount
  ],
  "userNumList": [
    { "id": 1, "value": 395 } // transferLoc
  ],
  "rgSession": 1
}
```
EDITTRAN - Edit existing transaction (expire existing transfer & create a new transfer)
	* transferLoc, sourceAccount, transferAmt, transferFrequency, endDate, day1, day2

### PowerOn Successful Response (EDITTRAN)
```jsonc
{
  "history": { // used for history event by node-poweron-proxy. stripped before being sent to client
   "change": {
    "application": "member-to-member-transfers-poweron",
    "name": "MemberToMemberTransferUpdated",
    "toMemberName": "John Smith",
    "amount": 1.00,
    "toAccountName": "Regular Shares",
    "fromAccountNumberMasked": "x00S0010",
    "toAccountNumberMasked": "x55S0001",
    "nextTransferDate":"08/07/2021",
    "frequency": "weekly", // included if frequency is not "once"
    "day1":"", // include dif frequency is not "once"
    "day2":"" // include dif frequency is not "once"
    }
  },
  "results": {
    "success": true,
    "transferLoc": "395",
    "currentState": "[PRELOADDATA]"
  }
}
```

### Client Request (DELETERECIP)

```jsonc
{
  "rgState": "DELETERECIP",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [],   
  "userNumList": [
    {"id": 1, "value": 395} // recipientLoc
  ],  
  "rgSession": 1
}
```
### PowerOn Successful Response (DELETERECIP)
```jsonc
  "results": {
    "success": true,
    "recipientLoc": 395,
    "currentState": "[PRELOADDATA]"
  }
```


### Client Request (DELETETRAN)

```jsonc
{
  "rgState": "DELETETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [],
  "userNumList": [
    { "id": 1, "value": 395 } // transferLoc
  ],  
  "rgSession": 1
}
```

### PowerOn Successful Response (DELETETRAN)
```jsonc
{
  "history": { // used for history event by node-poweron-proxy. stripped before being sent to client
   "change": {
    "application": "member-to-member-transfers-poweron",
    "name": "MemberToMemberTransferDeleted",
    "toMemberName": "John Smith",
    "amount": 1.00,
    "toAccountName": "Regular Shares",
    "fromAccountNumberMasked": "x00S0010",
    "toAccountNumberMasked": "x55S0001",
    "nextTransferDate":"08/07/2021",
    "frequency": "weekly", // included if frequency is not "once"
    "day1":"", // include dif frequency is not "once"
    "day2":"" // include dif frequency is not "once"
    }
  },
  "results": {
    "success": true,
    "transferLoc": 395,
    "currentState": "[PRELOADDATA]"
  }
}
```


### PowerOn Error Response (CREATETRAN,EDITTRAN,DELETERECIP,DELETETRAN)
```jsonc
{
  "results": {
    "errorCode": "5xx",
    "loggingErrorMessage": "error code description"
  }
}
```

### Error Codes
501 - Config file read / validation error
502 - Invalid source account
503 - Invalid recipient account
504 - Insufficient information
505 - Invalid input
506 - Error creating/deleting recipient
507 - Error creating/updating/deleting transfer
508 - Undefined error
509 - Member unverified

### Transfer Frequencies
The following strings are all valid transfer frequencies:
once, weekly, monthly, semiMonthly, biweekly, quarterly(?), yearly(?)

### Account types
The recipient member's account id is optional when creating a new transfer and when displaying saved recipients
available account types are savings checking, loan
