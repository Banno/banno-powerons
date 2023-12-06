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

| Error Code | Logging Error Message                                                                 |
| ---------- | ------------------------------------------------------------------------------------- |
| 500        | "Program running in memo mode" (The program was attempted while the system was in Memo Post Mode) |
| 501        | [No generic error logging message] (There was an error while attempting to read/process the configuration letter file.) *See notes for logging error message details for each specific configuration file error |
| 502        | "Ineligible Account" (The member's account type was listed as ineligible in the configuration Letter file) *See notes on possible extended logging error message details for this error code |
| 503        | "Error processing requested changes:" *See notes on multiple possible extended logging error message details for this error code |
| 504        | "Successful Address Update - Name LOC:xxxx" |

*Expanded Error Code Notes
- 501: “Ineligible Account”, it can have logging error message details for each of these specific configuration file errors:
	- "CFG open error "[with the config file name]":"[with system generated letter file open error message]
	-  "CFG read error "[with the config file name]":"[with system generated letter file read error message]
	- "Invalid Acct Types Setup"
	- "Invalid Warning Types Setup"
	- "Invalid Clear Warning Type Setup"
	- "Invalid Set Warning Type Setup"
	- "PO Box Types Setup Incomplete"
	- "No name types set for name level matching"
	- "Invalid test account found. Must be 10 digits long."
	- "No email detail lines specified for member email."
	- "Invalid 'from' email address specified for member email."
	- "Invalid email address specified for Conversations override."
	
 - 502: "Ineligible Account", it can also have the following extended logging error message details:
	 - ": Acct Warning xxxx"
	 - ": Acct Type xxxx"
	 
 - 503: "Error processing requested changes:", it can have the following extended logging error message details:
	 - "Could not locate name LOC:xxxxx"
	 - "Tracking 8 not found"
	 - "Tracking 8 without MbrAddr Link"
	 - "Missing data in required field"
		- Occurs when the new address does not have information in the street, city, state or zip code fields
	 - "Address entry exceeds Episys limits"
		- Occurs when the city, state and zip code fields combined exceeds 38 characters
	 - "Attempt to enter PO Box as street addr."
	 - "No allowed name records found"
	 - "Banno Chg Addr Err" &.....
		 -":  FM-"[with file maintenance System generated error message]
		 -" - Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]
		 -" - Shr Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]
		 -" - IRS Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]
		 -" - Ln Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]
		 -" - Pldg Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]
		 -" - EFT Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]	 
		 -" - Card Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]	
		 -" - X Ln Nm LOC xxxxxx  FM-"[with file maintenance System generated error message]	
