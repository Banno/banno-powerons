# CD Renew JSON contract

## Initial Request (PRELOADDATA):
```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.CD.RENEW.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789S0123"},
    {"id": 2, "value": ""},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```
 - userCharList[1]: 10-digit account number, 'S' and 4-digit share ID
	 - Represents the target certificate or club share


## Successful Response:
```json
{
  "results": {
    "currentState": {
      "name": "My Wonderful CD",
      "maturityDate": "01/01/21",
      "minimumBalance": 12345.67,
      "currentStatus": "Renew",
      "transferSLId": "1234567890S0010",
      "transferAmount": "12345.67"
    },
    "options": [{
        "name": "Increase certificate balance",
        "value": 1
      },
      {
        "name": "Change certificate term",
        "value": 2
      },
      {
        "name": "Transfer certificate balance",
        "value": 3
      },
      {
        "name": "Renew",
        "value": 4
      },
      {
        "name": "Disburse funds by check",
        "value": 5
      },
      {
        "name": "Suspend",
        "value": 6
      }
    ],
    "transferShares": [{
        "shareLoanId": "S0000",
        "shareLoanName": "MY PRIME SHARE",
        "shareLoanBal": "1234.56"
      },
      {
        "shareLoanId": "S0001",
        "shareLoanName": "MY SECONDARY SAVINGS",
        "shareLoanBal": "20000.00"
      }
    ]
  }
}
```
 - ***currentState:***
  	 - 1-**name**: Share nickname of targeted certificate/club share. If no nickname, share description.
  	 - 2-**maturityDate**: The current maturity date of the targeted certificate/club.
  	 - 3-**minimumBalance**: The current minimum balance requirement of targeted certificate/club.
  	 - 4-**currentStatus**: The current maturity option set for the targeted certificate/club.
  	 - 5-**transferSLID**: If the current maturity option includes transferring funds into or
  	 - out of the CD, this will be the current 10-digit account number and share/loan ID.
  	 - 6-**transferAmount**: If the current maturity option includes transferring funds into or
  	 - out of the CD, this will be the current amount to be transferred.

 
 - ***options***: comma-delimited list of available options to member (1 thru 6). Options listed will be based upon the allowable options configured by the CU.			 
     - 1-**Increase Certificate Balance**:  Add additional funds to the certificate and renew.
	 - 2-**Change Terms**: Roll funds over into a new certificate type (requires CU contact).
	 - 3-**Transfer**: 
		 - Full amount - all funds will be transferred out and certificate closed
		 - Partial amount - limited to min balance requirement. Certificate will be renewed
	 - 4-**Renew**:  Renew certificate to current terms.
	 - 5-**Withdraw by Check**: Withdraw balance by check, close certificate.
	 - 6-**Suspend**: Suspend certificate activity - funds retained in share.

 - ***transferShares***: a list of shares eligible for transferring certificate balance into or of 
      shares and loans as a source of funds for increasing certificate balance 
	 - 1-**shareLoanId**: 4-character share or loan ID
     - 2-**shareLoanName**: share nickname if available otherwise the share description
	 - 3-**shareLoanBalance**: the current share available balance


## Request (PROCESSDATA):
```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.CD.RENEW.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789S0123"}
  ],
  "userNumList": [
    {"id": 1, "value": 1},
    {"id": 2, "value": "123456.78"}
],
  "rgSession": 1
}
```
 - userNumList[1]: User selected option
	 - Represents the CD renewal option the member selected.
 - userNumList[2]: User selected option
	 - The transfer in/out amount
 - userCharList[1]: 10-digit account number, 'S' or 'L' and 4-digit share/Loan ID
	 - Represents the source or target share/loan of the transfer

## Successful Response:
```json
{
  "results": "success",
  "updatedState": {
    "name": "My Wonderful CD",
    "maturityDate": "01/01/21",
    "minimumBalance": 12345.67,
    "currentStatus": "Transfer",
    "transferSLId": "1234567890S0010",
    "transferAmount": "12345.67"
  }
}
```
 - ***updatedState***
  	 - 1-**name**: Share nickname of targeted certificate/club share. If no nickname, share description.
  	 - 2-**maturityDate**: The current maturity date of the targeted certificate/club.
  	 - 3-**minimumBalance**: The current minimum balance requirement of targeted certificate/club.
  	 - 4-**newStatus**: The new maturity option set for the targeted certificate/club.
  	 - 5-**transferSLID**: If the current maturity option includes transferring funds into or
  	 - out of the CD, this will be the current 10-digit account number and share/loan ID.
  	 - 6-**transferAmount**: If the current maturity option includes transferring funds into or
  	 - out of the CD, this will be the current amount to be transferred.


## Unsuccessful Response:
If any request is not successful for any number of reasons, or the desired action results in an error condition, the PowerOn will respond with the following error structure :

```json
{
  "errorCode": "XXX",
  "loggingErrorMessage": "[error code detail]"
}
```
## Error Codes

| errorCode   | loggingErrorMessage                                             |
|-------------|-----------------------------------------------------------------|
| 500         | Generic Error                                                   |
| 501         | Configuration file error (error opening, reading or processing) |
| 502         | Invalid account or share (based on account type/share warning)  |
| 503         | Maturity selection previously made - contact CU                 |
| 504         | Processing Error                                                |
| 505         | Cross account access attempt                                    |


