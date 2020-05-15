# ODTOPTIN JSON contract

NOTES: every state checks memoMode - if memoMode is true, we cannot proceed.

## PRELOADDATA
To get things started, the client will send the initial PRELOADDATA request.
  * userChrList[1] is 10-digit ACCOUNT:NUMBER+'S'+4-character SHARE:ID
```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [
    {"id": 1, "value": "1234567890S0010"},
    {"id": 2, "value": ""},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

Poweron response - Config File read error:
```json
{
  "memoMode": false,
  "results": {
     "errorCode":"501",
     "loggingErrorMessage": "Error reading from config file"
  }
}
```

Poweron response - If no eligible shares:
```json
{
  "memoMode": false,
  "results": {
     "errorCode":"502",
     "loggingErrorMessage": "No Eligible Shares"
  }
}
```
Poweron response - With eligible shares:
  * maxSharesExceeded = true if member has more eligible shares than the program
    can process (90)
  * shareDetail lists eligible shares with the first being the share value passed
    back in PRELOADDATA
```json
{
	"memoMode": false,
	"results": {
		"maxSharesExceeded": false,
		"shareDetail": [{
				"SID": "0000",
				"currentState": false
			},
			{
				"SID": "0010",
				"currentState": true
			}
		],
		"Terms": [
   "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"3-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"4-This is test verbiage4. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"5-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
		]
	}
}
```

## PROCESSDATA

UX returns updated state of each share
  * UserChrList [1-5] Comma delimited list of eligible shares and new values to
    be set using the following format: 4 character SHARE:ID+':'+ '0' (no ODT) or
    '1' (enroll in ODT).  i.e: '0010:1' = Share ID 0010 to be enrolled in ODT.
    There can be up to 18 shares in each userChrList.
```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [
    {"id": 1, "value": "0000:0,0010:1,0012:1,0014:0"},                                                 
    {"id": 2, "value": ""},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

```json
PowerOn Response if error condition
{
  "memoMode": false,
  "results": {
     "errorCode":"503",
     "loggingErrorMessage": "Error Processing Update: [error detail]"
  }
}
```

```json
PowerOn Response if successful
{
  "memoMode": false,
  "results": {
     "SuccessfulUpdate": true
  }
}
```

Error Codes
500 - MemoMode Error
501 - Config file read / validation error
502 - No eligible shares
503 - Error processing update
504 -
