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
    {"id": 5, "value": "Old Street Address"}],
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

 - 500: "Program running in memo mode"
	 - The program was attempted while the system was in Memo Post Mode
 - 501: "Banno Chg addr err: Cfg file invalid"
	 - There was an error while attempting to read/process the configuration Letter file.
 - 502: "Ineligible Account"
	 - The member's account type was listed as ineligible in the configuration Letter file
 - 503: Error processing requested changes
	 - "Could not locate name LOC:xxxxx"
	 - "Banno Chg Addr Err:" &.....
		 - [file maintenance System generated error message]
		 - "Tracking 8 not found"
		 - "Tracking 8 without MbrAddr Link"
		 - "Address entry exceeds Episys limits"
			 - Occurs when the city, state and zip code fields combined exceeds 38 characters
		 - "Attempt to enter PO Box as street addr."
 - 504: "Successful Address Update - Name LOC:xxxx"

