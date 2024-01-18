# ODT Opt-in JSON Contract

## PRELOADDATA state

### PowerOn Input

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [],
  "userNumList": [],
  "rgSession": 1
}
```

### PowerOn Response

**When system is in Memo Mode - The program will not proceed:**

```json
{
  "memoMode": true
}
```

**When error condition is encountered:**

```json
{
  "errorCode": "xxx",
  "loggingErrorMessage": "[error message detail]"
}
```

**Successful response:**

```json
{
  "memoMode": false,
  "results": {
    "maxSharesExceeded": false,
    "shareDetail": [
      {
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
    ],
    "servicesInstructions": [
      "Check the box for an account to opt-in to overdraft services.  Un-check the box for an",
      "",
      "account to opt-out of overdraft services."
    ],
    "optInInformationText": [
      "This account will be opted-in to overdraft services. The fee disclosure below applies."
    ],
    "optOutInformationText": [
      "This account will be opted-out from overdraft services."
    ]
  }
}
```

**Response detail:**

- memoMode: boolean - true/false. Is the system in memo mode?
- results
  - maxSharesExceeded: boolean - true/false. Did the program find more than 13 eligible shares for this member?
  - shareDetail
    - SID: Share ID [SHARE:ID]
    - name: Share description [SHARE:DESCRIPTION]
    - balance: Share available balance [SHARE:AVAILABLEBALANCE]
    - currentState: boolean - true/false. Is the share enrolled in ODT services
- terms: Custom Terms
- fee Disclosure: Custom fee disclosure
- revocationInstructions: Custom revocation instructions
- servicesInstuctions: Custom services instructions
- optInInformationText: Custom opt-in information text
- optOutInformationText: Custom opt-out information text

## PROCESSDATA state

### PowerOn Input

UX returns updated state of each share. Share IDs listed are to be enrolled into ODT services. Valid share IDs not explicitly listed are not to be enrolled into ODT services

```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.ODTOPTIN.V1.POW",
  "userChrList": [
    { "id": 1, "value": "0000,0010,0012,0014" },
    { "id": 2, "value": "" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

@RGUSERCHR[1-5]: Comma delimited list of 4-digit share IDs to be enrolled into ODT services. Each @RGUSERCHR can contain up to 26, 4-character share IDs for a maximum of 130.

### PowerOn Response

**When error condition is encountered:**

```json
{
  "errorCode": "xxx",
  "loggingErrorMessage": "[error message detail]"
}
```

**Successful response:**

```json
{
  "memoMode": false,
  "results": {
    "maxSharesExceeded": false, //'true' if the number of shares found exceeds processing capabilities (130 shares)
    "shareDetailUpdated": [
      {
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

**Response Detail:**

- Response detail will duplicate PRELOADDATA state response detail but with updated values

## Error Codes

*See the Modifier section for additional details.
 The Modifier is appended to the main Logging Error Message.

| Error Code | Logging Error Message                                                                 | Modifier                                                                             | Additional Notes As Needed                    |
| ---------- | ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| 500        | Program running in memo mode |||
| 501        | Error reading from config file | :Error Opening Letterfile [configuration file name]: [system generated letter file read error message] ||
|            || :Error Reading Letterfile [configuration file name]: [system generated letter file read error message] ||
| 502        | Config file validation error | :Invalid tracking type||
|            || :Invalid source code for SCT in CFG | In the CFG for SCT (source code 1 value if opt-in = true) there is an invalid value. Remove the value from the CFG not listed as valid in CFG comments. |
|            || :Invalid source code for SCF in CFG | In the CFG for SCF (source code 1 value if opt-in = false) there is an invalid value. Remove the value from the CFG not listed as valid in CFG comments. |
|            || :Invalid auth/fee option for AFT in CFG | In the CFG for AFT (Auth & fee 1 value if opt-in = true) there is an invalid value. Remove the value from the CFG not listed as valid in CFG comments. |
|            || :Invalid auth/fee option for AFF in CFG | In the CFG for AFF (Auth & fee 1 value if opt-in = false) there is an invalid value. Remove the value from the CFG not listed as valid in CFG comments. |
| 503        | No eligible shares. |||
| 504        | Ineligible Acct Type 1234 found |||
| 505        | Account warning 123 exists |||
| 506        | Error attempting to update share tracking | : [file maintenance system error message] ||
| 507        | Error updating source code & auth/fee fields | : [file maintenance system error message] ||
| 508        | Error updating share overdraft tolerance amount | : [file maintenance system error message] ||

- Individual error codes are for ease of researching issues.
- All error codes except for '503' will be returned to the UX as '500'
