# BANNO.CARD.LOC.MGMT.V1.POW JSON contract

#### **Memo Mode Processing**
As the PowerOn program principly modifies account tracking files, Should
the system be in memo mode, program processing will end and error code
500 will be returned. This applies to all states

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "System is currently in Memo Mode"
}
```

## GETPRELOADDATA STATE

### Client Request (GETPRELOADDATA)
#### Initial client request

```json
{
  "rgState": "GETPRELOADDATA",
  "powerOnFileName": "BANNO.CARD.LOC.MGMT.V1.POW",
  "userChrList": [],
  "userNumList": [],
  "rgSession": 1
}
```

### Successful Response (GETPRELOADDATA):
#### Return list of block override options and current state of each

```json
{
	"results": {
 "maximumDays": 14,
	"currentStatus": [{
				"index": "001",
				"location": "Colorado",
				"blocked": true,
				"expiration": "--/--/--"
			},
			{
				"index": "002",
				"location": "New Mexico",
				"blocked": true,
				"expiration": "--/--/--"
			},
   {
				"index": "002",
				"location": "Arizona",
				"blocked": false,
				"expiration": "08/24/20"
			}
		],
		"disclaimerText": ["disclaimer ",
                     "lines ",
                     "with ",
                     "spaces."]
	}
}
```

### Error Responses (GETPRELOADDATA):
#### Missing or invalid CFG Letterfile:

```json
{
  "errorCode": 501,
  "loggingErrorMessage": "Error processing Letterfile BANNO.CARD.BLOCK.V1.CFG"
}
```

#### Ineligible Account based on account or warning type:

```json
{
  "errorCode": 502,
  "loggingErrorMessage": "Ineligible account [warning xx or type xx]"
}
```

## PROCESSDATA STATE

### Client Request  (PROCESSDATA)
#### The member has made their selctions for the PowerOn to process
```json

{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.CARD.LOC.MGMT.V1.POW",
  "userChrList":[
    {"id": 1, "value": "01:082020,02,03:081520"},  /comma delimited list
    {"id": 2, "value": ""},                        /number with date = overidden
    {"id": 3, "value": ""},                        /number without date = blocked
    {"id": 4, "value": ""},                        /all 5 char values can be used
    {"id": 5, "value": ""}],                       /maximum of 65 elements
  "userNumList": [],
  "rgSession": 1
}
```

### Successful Response (PROCESSDATA):
#### Return list or block override options and current state of each
```json
{
	"results": {
		"success": true,
	 "updatedStatus": [{
				"index": "001",
				"location": "Colorado",
				"blocked": true,
				"expiration": "--/--/--"
			},
			{
				"index": "002",
				"location": "New Mexico",
				"blocked": true,
				"expiration": "--/--/--"
			},
   {
				"index": "002",
				"location": "Arizona",
				"blocked": false,
				"expiration": "08/24/20"
			}
		]
	}
}
```

### Unsuccessful Response (PROCESSDATA):
#### Return error code with detail
```json
{
  "errorCode": 503,
  "loggingErrorMessage": "Error processing request:"+[error]
}
```

### Error codes
| Code   | Description                        |
|--------|------------------------------------|
| 500    | System in memo mode                |
| 501    | CFG Letterfile processing error    |
| 502    | Ineligible Account (type/warning)  |
| 503    | Error processing requested changes |
