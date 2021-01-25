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
		"enforceLimits": true,
		"transferLimits": [{
			"institutionCount": "5",
			"institutionAmount": "10000.00",
			"memberLimitCount": "3",
			"memberLimitAmount": "5000.00",
			"memberActualCount": "1",
			"memberActualAmount": "500.00"
		}],
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
				"trnsferLoc": "215",
				"sourceAccount": "1234567890S0020",
				"accountName": "CHECKING",
				"transferAmt": "120.00",
				"recipientName": "Emily Fitch",
				"recipientMemberId": "9876543210",
				"recipientAccount": "[9876543210L0001,savings,checking]",
				"recipientNickname": "Emmy",
				"startDate":"07/31/21",
				"endDate":"12/31/2021",
				"transferFrequency": "weekly",
				"day1": "",
				"day2": "",
				"saveRecipient": false,
				"memo": "Internal Note created by member"
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
				"endDate":"12/31/2025",
				"transferFrequency": "semi-monthly",
				"day1": "3",
				"day2": "31",
				"saveRecipient": false,
				"memo": "Internal Note created by member"
			}
		],
		"savedRedipients": [{
				"recipientLoc": "5443231543",
				"recipientName": "Binsy Flomor",
				"recipientSLId": "9876543210L0001",
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
## PROCESS REQUEST STATE
### Client Request (PROCESS)

```json
{
  "rgState": "PROCESS",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "[request type]:[data]|[data]|[data]|[data]|[data]|[data]" },
    { "id": 2, "value": "[data]|[data]|[data]|[data]|[data]|[data]" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [], //  usernum[1-5] is unused
  "rgSession": 1
}
```
Request Type, Required data:
* CREATERECIP - Create new recipient
	* Account Name (nickname), Account (10-digit member number, S or L, SL ID)
* DELETERECIP - Delete existing recipient
	* recipientLoc
* CREATETRAN - Create new transfer with new or existing recipient
	* Existing Recipient:  recipientLoc
	* New Recipient: "0",  Account Name (nickname), Account (10-digit member number, S or L, SL ID), Recipient Nickname, save:[T/F]
	* All recipients: sourceAccount, transferAmt, transferFrequency, startDate, endDate, day1, day2, memo
* EDITTRAN - Edit existing transaction (expire existing transfer & create a new transfer)
	* transferLoc, sourceAccount, transferAmt, transferFrequency, endDate, day1, day2, memo
* DELETETRAN
	* transferLoc,

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