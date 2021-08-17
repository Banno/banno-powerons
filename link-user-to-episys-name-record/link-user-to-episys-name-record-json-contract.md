# Change Address JSON contract
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
    "loggingErrorMessage": "logging error message detail*"
}
```
The "logging error message detail" will be a different message based upon the logic scenario which trapped the request and will be one of the following:
- CREATETRACKING Logic Scenario #1 applied
- CREATETRACKING Logic Scenario #2 applied

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
    "loggingErrorMessage": "logging error message detail*"
}
```
The "logging error message detail" will be a different message based upon the logic scenario which trapped the request and will be one of the following:
- LINKTRACKING Logic Scenario #0 applied
- LINKTRACKING Logic Scenario #1.a applied
- LINKTRACKING Logic Scenario #1.b applied
- LINKTRACKING Logic Scenario #2.a applied
- LINKTRACKING Logic Scenario #2.b applied
- LINKTRACKING Logic Scenario #3.a applied
- LINKTRACKING Logic Scenario #3.b applied
- LINKTRACKING Logic Scenario #3.c.1 applied
- LINKTRACKING Logic Scenario #3.c.2 applied
- LINKTRACKING Logic Scenario #3.c.3.a applied
- LINKTRACKING Logic Scenario #3.c.3.b applied
- LINKTRACKING Logic Scenario #3.c.4 applied

## Note:
*The "loggingErrorMessage" and related detail are not used by the UX and are only output for purposes of more easily determining which logic scenario was used by the program for a given request to assist with
troubleshooting individual case scenarios.
