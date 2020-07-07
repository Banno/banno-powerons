# Open subshare JSON contract

NOTES: every state checks memoMode - if memoMode is true, we cannot proceed.

## PRELOADDATA
To get things started, the client will send the initial PRELOADDATA request.
```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [],
  "userNumList": [],
  "rgSession": 1
}
```

Poweron response - when system is in Memo Mode:
```json
{
  "memoMode": true
}
```

Poweron response - Config File read error:
```json
{
  "errorCode":"501",
  "loggingErrorMessage": "Error reading from config file:"+[detail]
}
```

Poweron response - Config File validation error:
```json
{
  "errorCode":"502",
  "loggingErrorMessage": "Config file validation error"
}
```

Poweron response - If no eligible shares:
```json
{
  "errorCode":"503",
  "loggingErrorMessage": "No Eligible Shares"
}
```

Poweron response - Invalid Account type error:
```json
{
   "errorCode":"504",
   "loggingErrorMessage": "Ineligible account type [account type] found"
}
```

Poweron response - Account warning code error:
```json
{
   "errorCode":"505",
   "loggingErrorMessage": "Account warning [acount warning] exists"
}
```

Poweron response - With eligible shares:
```json
{
 "memoMode": false,
 "results": {
  "maxSharesExceeded": false, //'true' if the number of shares found exceeds processing capbilities (130 shares)
  "shareDetail": [{           //note: the first share returned will be the share the member
    "SID": "0000",            //currently has opened, provided it is eligible. All other shares will be in heirarchal order
    "name": "My Primary Share",
    "balance": "######9.99",
    "currentState": false
   },
   {
    "SID": "0010",
    "name": "SuperDuper Checking",
    "balance": "######9.99",
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
    {"id": 1, "value": "0000,0010,0012,0014"},  //comma dilineated list of 4-digit share ID for those shares which are
    {"id": 2, "value": ""},                     //to be enrolled into ODT services. All 5 userChr lists can be used
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

PowerOn Response if error while updating tracking record
```json
{
  "errorCode":"506",
  "loggingErrorMessage": "Error attempting to update share tracking: [error detail]"
}
```

PowerOn Response if error while updating share record
```json
{
  "errorCode":"507",
  "loggingErrorMessage": "Error updating sourcecode & auth/fee fields: [error detail]"
}
```

PowerOn Response if successful
```json
{
 "memoMode": false,
 "results": {
  "maxSharesExceeded": false,  //'true' if the number of shares found exceeds processing capbilities (130 shares)
  "shareDetailUpdated": [{
    "SID": "0000",
    "name": "My Primary Share",
    "balance": "######9.99",
    "currentState": false
   },
   {
    "SID": "0010",
    "name": "SuperDuper Checking",
    "balance": "######9.99",
    "currentState": true
   }
  ]
 }
}
```

Error Codes // Individual error codes are for ease of researching issues.
            // All error codes except for '503' will be returned to the UX as '500'

501 - Config file read error
502 - Config file validation error
503 - No eligible shares
504 - Ineligible account type
505 - Account warning found
506 - Error attempting to update share tracking record
507 - Error updating source code and auth & fee fields
