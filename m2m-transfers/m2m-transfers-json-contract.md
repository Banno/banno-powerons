#JSON contract for BANNO.M2MTRANSFERS.V1.POW

## GET PRELOADDATA STATE
### Client Request (PRELOADDATA)

```jsonc
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890" }, // 10-digit account
    { "id": 2, "value": "" }, // userchar[2-5] is unused
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
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
      "enforceLimits": true,// remaining transferLimit properties are optional, if enforceLimits is false
      "countLimit": "5", 
      "memberCount": "1",
      "amountLimit": "1000.00",
      "memberAmount": "100.00",
      "perTransferLimit": "500.00"
    },
    "availableShares": [
      {
        "transferSLId": "0000800005S0002",
        "transferName": "SUMMER SAVER",
        "available": "50.00"
      },
      {
        "transferSLId": "0000800005S0010",
        "transferName": "REGULAR CHECKING",
        "available": "500.00"
      },
      {
        "transferSLId": "0000800005S0015",
        "transferName": "ULTIMATE CHECKING (10)",
        "available": "0.00"
      }
    ],
    "scheduledTransfers": [
      {
        "transferLoc": "215",
        "sourceAccount": "1234567890S0020",
        "accountName": "CHECKING",
        "transferAmt": "120.00",
        "recipientName": "Emily Fitch",
        "recipientMemberId": "9876543210",
        "recipientAccountId": "S0001", // could be specific account, or "savings" or "checking"?
        "recipientNickname": "Emmy",
        "startDate": "07/31/2021",
        "endDate": "12/31/2021", // unused
        "transferFrequency": "weekly", // daily, weekly, monthly, yearly, semiMonthly, biweekly, quarterly
        "day1": "", // semi-monthly - first day
        "day2": "" // semi-monthly - second day
      },
      {
        "transferLoc": "395",
        "sourceAccount": "1234567890S0001",
        "accountName": "SAVINGS",
        "transferAmt": "120.00",
        "recipientName": "Howard Cleo",
        "recipientMemberId": "9876543210",
        "recipientAccountId": "S0001",
        "recipientNickname": "Howie",
        "nextTransferDate": "12/3/2020",
        "startDate": "07/03/2021",
        "endDate": "12/31/2025", // unused
        "transferFrequency": "semi-monthly",
        "day1": "3",
        "day2": "31"
      }
    ],
    "savedRedipients": [
      {
        "recipientLoc": "5443231543",
        "recipientName": "Binsy Flomor",
        "recipientSLId": "9876543210L0001", // full account id or "checking" or "savings"?
        "recipientNickName": "Zeke's Future"
      },
      {
        "recipientLoc": "5431543",
        "recipientName": "Kylie Crenshaw",
        "recipientSLId": "9876543210L0001",
        "recipientNickName": "Sally Martin"
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
## CREATE TRANSFER REQUEST STATE
### Client Request (CREATETRAN)

```jsonc
{
  "rgState": "CREATETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890S0001|9876543210L0001|weekly|12/31/2021|1|15" },  // [sourceAccount][destinationAccount][frequency]|[startDate]|[day1]|[day2]
    { "id": 2, "value": "HUB|nickname" }, // [first 3][nickname]
    { "id": 3, "value": "internal memo for immediate transfers" }, // internal memo for immediate transfers (max 132 characters)
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
    { "id": 1, "value": 395 }, // recipientLoc if available
    { "id": 2, "value": 1000.51 } // transferAmt
  ],
  "rgSession": 1
}
```
* CREATETRAN - Create new transfer with new or existing recipient
	* Existing Recipient:  recipientLoc will be present
	* New Recipient: if nickname is present, create new
	* All recipients: sourceAccount, destinationAccount, transferAmt, transferFrequency, startDate, endDate, day1, day2
	* One-time immediate transfers have an optional internal memo field.

### Client Request (EDITTRAN)

```jsonc
{
  "rgState": "EDITTRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "weekly|12/31/2021|1|15" }, // [transferFrequency]|[endDate]|[day1]|[day2] max 132 characters
    { "id": 2, "value": "internal memo for immediate transfers" }, // internal memo for immediate transfers only
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
    { "id": 1, "value": 395 }, // transferLoc
    { "id": 2, "value": 1000.51 } // transferAmt
  ],
  "rgSession": 1
}
```
EDITTRAN - Edit existing transaction (expire existing transfer & create a new transfer)
	* transferLoc, sourceAccount, transferAmt, transferFrequency, endDate, day1, day2

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

### PowerOn Successful Response (CREATETRAN,EDITTRAN,DELETERECIP,DELETETRAN)
```jsonc
{
  "results": {
    "success": true,
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
500 - System in Memo Mode
501 - Config file read / validation error
502 - Invalid source account
503 - Invalid recipient account
504 - Insufficient information
505 - Invalid input
506 - Error creating/deleting recipient
507 - Error creating/updating/deleting transfer
508 - Undefined error
