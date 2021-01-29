#JSON contract for BANNO.M2MTRANSFERS.V1.POW

## GET PRELOADDATA STATE
### Client Request (PRELOADDATA)

```json
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

```json
{
	"currentState": {
		"transferLimits": {
			"enforceLimits": true,
			"allowTransfers": true,
			"countLimit:" "5",// optional if enforceLimits is false
			"memberCount": "1",
			"amountLimit": "1000.00",
			"memberAmount": "100.00",
			"perTransferLimit": "500.00"
		},
		"availableShares": [{
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

		"scheduledTransfers": [{
				"transferLoc": "215",
				"sourceAccount": "1234567890S0020",
				"accountName": "CHECKING",
				"transferAmt": "120.00",
				"recipientName": "Emily Fitch",
				"recipientMemberId": "9876543210",
				"recipientAccount": "9876543210L0001", // could be specific account, or "savings" or "checking"?
				"recipientNickname": "Emmy",
				"startDate": "07/31/21",
				"endDate": "12/31/2021", // unused
				"transferFrequency": "weekly", // daily, weekly, monthly, yearly, semiMonthly, biweekly, quarterly
				"day1": "", // semi-monthly - first day
				"day2": ""// semi-monthly - second day
				
			},
			{
				"transferLoc": "395",
				"sourceAccount": "1234567890S0001",
				"accountName": "SAVINGS",
				"transferAmt": "120.00",
				"recipientName": "Howard Cleo",
				"recipientMemberId": "9876543210",
				"recipientShareId": "9876543210L0001",
				"recipientNickname": "Howie",
				"nextTransferDate": "12/15/2020",
				"startDate":"07/03/21",
				"endDate":"12/31/2025", // unused
				"transferFrequency": "semi-monthly",
				"day1": "3",
				"day2": "31"
			}
		],
		"savedRedipients": [{
				"recipientLoc": "5443231543",
				"recipientName": "Binsy Flomor",
				"recipientSLId": "9876543210L0001",// full account id or "checking" or "savings"?
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
```json
{
  "errorCode": "501",
  "loggingErrorMessage": "Error [opening/reading] from config file: [error msg]"
}
```
#### PowerOn response - Configuration File read error:
```json
{
  "errorCode": "501",
  "loggingErrorMessage": "Error [opening/reading] from config file: [error msg]"
}
```
### PowerOn response - Invalid Account (by Account Warning):

```json
{
  "errorCode": "502",
  "loggingErrorMessage": "Invalid Account - account warning xxx"
}
```
### PowerOn response - No Eligible Share(s) found:

```json
{
  "errorCode": "503",
  "loggingErrorMessage": "No eligible shares found"
}
```
## CREATE TRANSFER REQUEST STATE
### Client Request (CREATETRAN)

```json
{
  "rgState": "CREATETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890S0001" },  // sourceAccount
    { "id": 2, "value": "weekly|12/31/2021|1|15" }, // [transferFrequency]|[endDate]|[day1]|[day2] max 132 characters
    { "id": 3, "value": "SAVINGS|9876543210L0001|Zeke's Future" }, // Account Name (nickname), Account (10-digit member number, S or L, SL ID), Recipient Nickname
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
	{ "id": 1, "value": 1000.51 } // transferAmt
	{ "id": 2, "value": 395 } // transferLoc
	{ "id": 3, "value": 0 } // recipientLoc '0' if new account
	],
  "rgSession": 1
}
```
* CREATETRAN - Create new transfer with new or existing recipient
	* Existing Recipient:  recipientLoc
	* New Recipient: "0",  Account Name (nickname), Account (10-digit member number, S or L, SL ID), Recipient Nickname
	* All recipients: sourceAccount, transferAmt, transferFrequency, startDate, endDate, day1, day2

### Client Request (EDITTRAN)

```json
{
  "rgState": "EDITTRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890S0001" },  // sourceAccount
    { "id": 2, "value": "weekly|12/31/2021|1|15" }, // [transferFrequency]|[endDate]|[day1]|[day2] max 132 characters
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
	{ "id": 1, "value": 1000.51 } // transferAmt
	{ "id": 2, "value": 395 } // transferLoc
  ],
  "rgSession": 1
}
```
EDITTRAN - Edit existing transaction (expire existing transfer & create a new transfer)
	* transferLoc, sourceAccount, transferAmt, transferFrequency, endDate, day1, day2

### Client Request (DELETERECIP)

```json
{
  "rgState": "DELETERECIP",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [], //  userChrList[1-5] is unused
  "userNumList": [
	  {"id": 1, "value": 5443231543} // recipientLoc
	  ],  
  "rgSession": 1
}
```

### Client Request (DELETETRAN)

```json
{
  "rgState": "DELETETRAN",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [], //  userChrList[1-5] is unused
  "userNumList": [
	   { "id": 1, "value": 395 } // transferLoc
  ],  
  "rgSession": 1
}
```

### PowerOn Successful Response (PROCESS)
```json
{
	"results": "success",
	[PRELOADDATA]
}
```
### PowerOn Error Response (PROCESS)
```json
{
	"results": [
		"errorCode",
		"5xx",
		"error code description"
	],
	[PRELOADDATA]
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
506 - Undefined error
