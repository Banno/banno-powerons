# Change Address JSON contract
## PROCESSADDRDATA state
### PowerOn Input
```json
{
  "rgState": "PROCESSADDRDATA",
  "powerOnFileName": "BANNO.CHANGE.ADDR.V1.POW",
  "userChrList":[
    {"id": 1, "value": "Street Address|Extra Address"},
    {"id": 2, "value": "City|State|Zip Code"},
    {"id": 3, "value": "Member Note"},
    {"id": 4, "value": "Member's Banno ID"},
    {"id": 5, "value": "Old Street Address|Old Extra Address"}],
  "userNumList": [],
  "rgSession": 1
}
```
### PowerOn Response
```json
{
    "responseCode": "xxx",
    "loggingErrorMessage": "logging error message detail"
}
```
### Response Codes & Corresponding Messages

*See Modifier section for additional details

| Error Code | Logging Error Message                                                                 | Modifier                                                       | Notes
| ---------- | ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| 500        | Program running in memo mode |||
| 501        | Invalid Acct Types Setup || This error represents issues with the configuration letter file setup |
|            | Invalid Warning Types Setup |||
|            | Invalid Clear Warning Type Setup |||
|            | Invalid Set Warning Type Setup |||
|            | PO Box Types Setup Incomplete |||
|            | No name types set for name level matching |||
|            | Invalid test account found. Must be 10 digits long. |||
|            | No email detail lines specified for member email. |||
|            | Invalid 'from' email address specified for member email. |||
|            | Invalid email address specified for Conversations override. |||
|            | CFG open error [config file name]:[system generated letter file open error message] |||
|            | CFG read error [config file name]:[system generated letter file read error message] |
| 502        | Ineligible Account || The member's account type was listed as ineligible in the configuration letter file|
|            |                    | : Acct Warning xxxx ||
|            |                    | : Acct Type xxxx ||
| 503        | Error processing requested changes: |||
|            || Could not locate name LOC:xxxxx ||
|            || Tracking 8 not found ||
|            || Tracking 8 without MbrAddr Link ||
|            || Missing data in required field | Occurs when the new address does not have information in the street, city, state or zip code fields|
|            || Address entry exceeds Episys limits | Occurs when the city, state and zip code fields combined exceeds 38 characters |
|            || Attempt to enter PO Box as street addr. ||
|            || No allowed name records found ||
|            || Banno Chg Addr Err:  FM-[file maintenance system error message]||
|            || Banno Chg Addr Err - Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - Shr Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - IRS Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - Ln Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - Pldg Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - EFT Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - Card Nm LOC xxxxxx  FM-[file maintenance system error message] ||
|            || Banno Chg Addr Err - X Ln Nm LOC xxxxxx  FM-[file maintenance system error message] ||
| 504        | Successful Address Update - Name LOC:xxxx |||
|            | Override email error: [Email send error] || Error when conversations override is enabled |
|            | Conversations override email sent to [Param Override Email Address] || Error when conversations override is enabled |

