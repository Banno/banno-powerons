#JSON contract for BANNO.M2MTRANSFERS.V1.POW
## GET PRELOADDATA STATE
### Client Request (PRELOADDATA)
```jsonc
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [], // userchar[1-5] is unused
  "userNumList": [{ "id": 1, "value": 3 }], //  ux version #
  "rgSession": 1
}
```
### PowerOn Successful Response (PRELOADDATA)
list of institution and member limits, eligible shares, list of scheduled transfers, list of saved recipients
```jsonc
{
  "currentState": {
    "slidLength":4,
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
        "nextTransferDate":"08/07/2021",
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
    { "id": 1, "value": "9876543210|HUB|accountType|0234" }// [member id][first 3 of last name or business name][accountType][accountId]
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
    { "id": 1, "value": "1234567890S0001|9876543210|L0001|weekly|12/31/2021|1|15" },  // [sourceAccount][recipient member id][account type or id][frequency]|[startDate || "soonest" ]|[day1]|[day2]
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

*See the Modifier section for additional details.
 The Modifier is appended to the main Logging Error Message.

| Error Code | Logging Error Message                                                                 | Modifier                                                                             | Additional Notes As Needed                    |
| ---------- | ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| 500        | Program running in memo mode |||
| 501        | Config file error | : [configuration file name] open error - [system generated letter file read error message] ||
|            || : [configuration file name] read error - [system generated letter file read error message] ||
|            || : 3-Error Reading Letterfile [configuration file name]: [system generated letter file read error message] ||
|            || : Invalid Parameter in CFG file ||
| 502        | Invalid Source Account | : Pref Access type 3 not found ||
|            || : Acct Warning 1234 ||
|            || : No eligible transfer from shares/loans ||
|            || : Acct Type 1234 ||
| 503        | Invalid Recipient Account |||
| 504        | Invalid Source Account | Cannot calc member limits. ||
| 505        | Invalid Input |||
| 506        | Error Processing Recipient Record || Error creating/deleting recipient |
| 507        | Error Processing Transfer Record | No Modifier. Error Code 507 does not always provide a Modifier. | This set of errors is for creating/updating/deleting transfers |
|            || Target s/l xfer Loc 1234567 not found ||
|            || : Target ID not found or invalid ||
| 508        | Undefined Error |||
| 509        | Member verification failed | : Account Closed - 99/99/99 | This set of errors is for member unverified |
|            || : Name verification failed ||
|            || : No valid share or loan found ||
|            || : Target S/L not found ||
|            || : S/L closed or charged-off - 99/99/99 ||
| 510        | Account ID incorrect |||
| 511        | Request exceeds limits |||

### Transfer Frequencies

*The following strings are all valid transfer frequencies:*
- once
- weekly
- monthly
- semiMonthly
- biweekly
- quarterly
- yearly

### Account types

The recipient member's account id is optional when creating a new transfer and when displaying saved recipients

*Available account types are:*
- savings
- checking
- loan
