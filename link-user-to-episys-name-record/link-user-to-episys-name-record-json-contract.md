# Database Check JSON contract
## CREATETRACKING state
### PowerOn Input
```json
{
  "rgState": "CREATETRACKING",
  "powerOnFileName": "BANNO.DATABASE.CHECK.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0,1,2,3,4"},
    {"id": 2, "value": "da5xx2a-4xx1-4xxe-9725-d8xxxxxxxxaf"},
    {"id": 3, "value": "20211270000000003"},
    {"id": 4, "value": "20211270000000004"}],
  "userNumList": [
    {"id": 1, "value": 1234}]
  "rgSession": 1
}
```
 - userCharList[1]: Comma delimited list of valid name types or "all"
 - userCharList[2]: Banno logged-in user ID
 - userCharList[3]: Member number link (from linked name record)
 - userCharList[4]: Member address number link (from linked name record)
 - userNumList[1]: Preference record locator
### PowerOn Response
```json
{
    "results": "COMPLETE" or "COMPLETE WITH ERROR",
    "scenario": "scenario message detail*",
    "debugMode": false
}

```
The "scenario message detail" will be a different message based upon the logic scenario which trapped the request and will be one of the following:
| Scenario                                       | Additional Notes As Needed                                                            |
| ---------------------------------------------- | ------------------------------------------------------------------------------------- |
| CREATETRACKING Logic Scenario #1 applied       | Scenario #1: Check for existing tracking with matching ID and Pref Loc, return SUCCESS |
| CREATETRACKING Logic Scenario #2 applied       | Scenario #2: Tracking 8 with matching ID and Pref Loc not found, Create tracking |

### PowerOn Response for Error Code 500 and 501
```json
{
    "errorCode": 999,
    "loggingErrorMessage": "logging error message detail*"
}
```

## LINKTRACKING state
### PowerOn Input
```json
{
  "rgState": "LINKTRACKING",
  "powerOnFileName": "BANNO.DATABASE.CHECK.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0,1,2,3,4"},
    {"id": 2, "value": "da5xx2a-4xx1-4xxe-9725-d8xxxxxxxxaf"}],
  "userNumList": [
    {"id": 1, "value": 1234}]
  "rgSession": 1
}
```
 - userCharList[1]: Comma delimited list of valid name types or "all"
 - userCharList[2]: Banno logged-in user ID
 - userNumList[1]: Preference record locator
### PowerOn Response
```json
{
    "results": "COMPLETE" or "COMPLETE WITH ERROR",
    "scenario": "scenario message detail*",
    "debugMode": true
}

```
## Scenario Logging

The "scenario message detail" will be a different message based upon the logic scenario which trapped the request and will be one of the following:
| Scenario                                       | Additional Notes As Needed                                                            |
| ---------------------------------------------- | ------------------------------------------------------------------------------------- |
| LINKTRACKING Logic Scenario #0 applied         | Scenario #0: Update the existing tracking record with the Banno member ID passed back, if has existing tracking with matching Pref locator code |
| LINKTRACKING Logic Scenario #1.a applied       | Scenario #1.a: If only 1 name record with valid name type, create tracking 08 with Banno user #, Preference LOC, the member link number, and the address link number. |
| LINKTRACKING Logic Scenario #1.b applied       | Scenario #1.b: If more than 1 name record with valid name type, create tracking with Banno user #, Preference LOC.  Return COMPLETE WITH ERROR and add note "multiple valid name records found" |
| LINKTRACKING Logic Scenario #2.a applied       | Scenario #2.a: w/1 valid name rec: revise tracking by changing Preference LOC, and adding the member link number and the address link number (if available). |
| LINKTRACKING Logic Scenario #2.b applied       | Scenario #2.b: w/more than 1 valid name rec: revise tracking by changing Preference LOC and clearing out both the member and the address number links. Return COMPLETE WITH ERROR and add note "multiple valid name records found" |
| LINKTRACKING Logic Scenario #3.a applied       | Scenario #3.a: If both member link and address link number are blank AND only 1 name record with valid name type, revise tracking 08 with the member link number and the address link number (if available). |
| LINKTRACKING Logic Scenario #3.b applied       | Scenario #3.b: If both member link and address link numbers are blank AND more than 1 name record with valid name type, return COMPLETE WITH ERROR and add note "multiple valid name records found |
| LINKTRACKING Logic Scenario #3.c.1 applied     | Scenario #3.c.1: If both numbers are found in a single name record, return success. |
| LINKTRACKING Logic Scenario #3.c.2 applied     | Scenario #3.c.2: If no name records with either link numbers, remove both numbers from tracking 08. Return COMPLETE WITH ERROR and add note "invalid name links removed from tracking 08" |
| LINKTRACKING Logic Scenario #3.c.3.a applied   | Scenario #3.c.3.a: If only member link matches & If new address link found (address link in name record is different than the address link in the tracking record), revise tracking 08 with the address link from the name record. Return success. |
| LINKTRACKING Logic Scenario #3.c.3.b applied   | Scenario #3.c.3.b: If no new address link # found, revise tracking 08 blanking out address link field. Return COMPLETE WITH ERROR and add note "invalid address link removed from tracking 08" |
| LINKTRACKING Logic Scenario #3.c.4 applied     | Scenario #3.c.4: If only address link matches, revise tracking 08 blanking out both member link and address link fields.  Return COMPLETE WITH ERROR and add note "invalid member and address links removed from tracking 08" |
| LINKTRACKING uncoded scenario                  | Uncoded scenario catch |

### PowerOn Response for Error Code 500 and 501
```json
{
    "errorCode": 999,
    "loggingErrorMessage": "logging error message detail*"
}
```
## Error Codes

| Error Code | Logging Error Message                                                                 |
| ---------- | ------------------------------------------------------------------------------------- |
| 500        | Program running in memo mode |
| 501        | No eligible name types defined |

## Note:
*The "loggingErrorMessage" and "scenario" are not used by the UX and are only output for purposes of more easily determining which logic scenario was used by the program for a given request to assist with
troubleshooting individual case scenarios.
