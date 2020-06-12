# Open subshare JSON contract

NOTES: every state checks memoMode - if memoMode is true, we cannot proceed.

## PRELOADDATA
To get things started, the client will send the initial PRELOADDATA request.
```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [
    {"id": 1, "value": "1234567890S0010"},  [NOTE: 10-digit account+'S'+SHARE:ID]
    {"id": 2, "value": ""},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

Poweron response - when system is in Memo Mode:
```json
{
  "memoMode": true,
}
```

Poweron response - Config File read error:
```json
{
  "errorCode":"501",
  "loggingErrorMessage": "Error reading from config file:"+[detail]
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
```json
{
 "memoMode": false,
 "results": {
  "maxSharesExceeded": false, ['true' if the number of shares found exceeds processing capbilities (130 shares)]
  "shareDetail": [{           [note: the first share returned will be the share the member]
    "SID": "0000",            [currently has opened, provided it is eligible. All other shares will be in heirarchal order]
    "name": "My Primary Share"
    "balance": "######9.99"
    "currentState": false
   },
   {
    "SID": "0010",
    "name": "SuperDuper Checking"
    "balance": "######9.99"
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
   "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
   "2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
   "3-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
   "4-This is test verbiage4. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
   "5-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
  ],
  "revocationInstructions": [
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
```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [
    {"id": 1, "value": "0000,0010,0012,0014"},  [comma dilineated list of 4-digit share ID for those shares which are]
    {"id": 2, "value": ""},                     [to be enrolled into ODT services. All 5 userChr lists can be used]
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

PowerOn Response if error condition
```json
{
  "memoMode": false,
  "errorCode":"504",
  "loggingErrorMessage": "Error Processing Update(s): [error detail]"
}
```

PowerOn Response if successful
```json
{
 "memoMode": false,
 "results": {
  "maxSharesExceeded": false,
  "shareDetailUpdated": [{
    "SID": "0000",
    "name": "My Primary Share"
    "balance": "######9.99"
    "currentState": false
   },
   {
    "SID": "0010",
    "name": "SuperDuper Checking"
    "balance": "######9.99"
    "currentState": true
   }
  ]
 }
}
```

Error Codes (individual error codes are for ease of researching issues.
             All error codes will be returned to the UX as '500')
500 - MemoMode Error
501 - Config file read / validation error
502 - Invalid Account error:
503 - No eligible shares
504 - Error processing update
