# ODTOPTIN JSON contract

NOTES: * Every state checks memoMode - if memoMode is true, we cannot proceed.
       * All errors returned as error code 500, other error codes as listed below
         are for message return configuration only.

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

Poweron response - Memo Mode error:
```json
{
  "memoMode": true,
}
```

Poweron response - Config File read error:
```json
{
  "errorCode":"501",
  "loggingErrorMessage": "Error reading from config file:"+[readerror]
}
```

Poweron response - Invalid Account error:
```json
{
   "errorCode":"502",
   "loggingErrorMessage": "Invalid Account:"+[Invalid Account Type XX, Acct Warning XX, Acct Svc Code XX]
}
```

Poweron response - If no eligible shares:
```json
{
   "errorCode":"503",
   "loggingErrorMessage": "No Eligible Shares"
}
```

Poweron response - With eligible shares:
  * maxSharesExceeded = true if member has more eligible shares than the program
    can process (180)
  * shareDetail lists eligible shares with the first being the share value passed
    back in PRELOADDATA. All other shares returned will be in hierarchical order
```json
{
	"memoMode": false,
	"results": {
		"maxSharesExceeded": [false,true],
		"shareDetail": [{
				"SID": "0000",
				"name": "Savings",
				"availBalance": "12345.67",
				"currentState": false
			},
			{
				"SID": "0010",
				"name": "My Personal Checking",
				"availBalance": "876.54",
				"currentState": true
			}
		],
		"terms": [
                        "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"3-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"4-This is test verbiage4. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
			"5-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
		],
		"feeDisclosure": [
                        "1-This is test fee disclosure.  This is test fee disclosure. This is test fee disclosure. This is test fee disclosure.",
			"2-This is test fee disclosure.  This is test fee disclosure. This is test fee disclosure. This is test fee disclosure.",
			"3-This is test fee disclosure.  This is test fee disclosure. This is test fee disclosure. This is test fee disclosure.",
			"4-This is test fee disclosure.  This is test fee disclosure. This is test fee disclosure. This is test fee disclosure.",
			"5-This is test fee disclosure.  This is test fee disclosure. This is test fee disclosure. This is test fee disclosure."
		]
	}
}
```

## PROCESSDATA

UX returns list of shares to be enrolled in ODT.
  * UserChrList [1-5] Comma delimited list of ONLY those shares (by ID) which the
    member wishes to be enrolled. All other eligible shares not part of this return
    will be assumed to be opted out of ODT. There may be up to 26 shares in each
    userChrList.
```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [
    {"id": 1, "value": "0000,0010,0012,0014,....."},                                                 
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
   "errorCode":"504",
   "loggingErrorMessage": "Error Processing Update: [error detail]"
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
502 - Invalid Account
503 - No ineligible Shares
504 - Error processing updates
