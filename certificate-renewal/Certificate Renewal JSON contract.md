# CD Renew JSON contract

## Initial Request (PRELOADDATA):
```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.CD.RENEW.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789S0123"}
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
      "minimumBalance": "12345.67",
      "currentStatus": "Renew",
      "currentTerm": "24 months",
      "transfers": [{
          "transferSLId": "0000800005S0002",
          "transferAmount": "null",
          "transferPercent": "50.000"
        },
        {
          "transferSLId": "0000800005S0010",
          "transferAmount": "500.00",
          "transferPercent": "null"
        },
        {
          "transferSLId": "0000800005S0000",
          "transferAmount": "0.00",
          "transferPercent": "null"
        }
      ]
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
    "transferInOut": [{
        "shareLoanId": "1234567890S0000",
        "shareLoanName": "MY PRIME SHARE",
        "shareLoanBal": "1234.56"
      },
      {
        "shareLoanId": "1234567890S0001",
        "shareLoanName": "MY SECONDARY SAVINGS",
        "shareLoanBal": "20000.00"
      }
    ],
    "payeeAddress": [
      "addressline1",
      "addressline2",
      "addressline3",
      "addressline4",
      "addressline5",
      "addressline6"
    ],
    "payeeTerms": [
      "termsLine1",
      "termsLine2",
      "termsLine3",
      "termsLine4",
      "termsLine5",
      "termsLine6"
    ],
    "suspendMessage": [
      "suspendLine1",
      "suspendLine2",
      "suspendLine3",
      "suspendLine4",
      "suspendLine5",
      "suspendLine6"
    ],
    "reviewMessage": [
      "reviewLine1",
      "reviewLine2",
      "reviewLine3",
      "reviewLine4",
      "reviewLine5",
      "reviewLine6"
    ]
  }
}
```
 - ***currentState:***
  	 - 1-**name**: Share nickname of targeted certificate/club share. If no nickname, share description.
  	 - 2-**maturityDate**: The current maturity date of the targeted certificate/club.
  	 - 3-**minimumBalance**: The current minimum balance requirement of targeted certificate/club.
  	 - 4-**currentStatus**: The current maturity option set for the targeted certificate/club.
  	 - 5-**currentTerm**: The current term of the targeted certificate/club.
  	 - 6-**transferSLID**: If the current maturity option includes transferring funds into or out of the CD, this will be the current 10-digit account number and share/loan ID.
  	 - 7-**transferAmount**: If the current maturity option includes transferring funds into or out of the CD, this will be the current amount to be transferred.

  - ***options***: comma-delimited list of available options to member (1 thru 6). Options listed will be based upon the allowable options configured by the CU.			 
	 - 1-**Increase certificate balance**:  Add additional funds to the certificate and renew.
	 - 2-**Change certificate term**: Roll funds over into a new certificate type (requires CU contact).
	 - 3-**Transfer certificate balance**: 
		 - Full amount - all funds will be transferred out and certificate closed
		 - Partial amount - limited to min balance requirement. Certificate will be renewed
	 - 4-**Renew**:  Renew certificate to current terms.
	 - 5-**Disburse funds by check**: Withdraw balance by check, close certificate.
	 - 6-**Suspend**: Suspend certificate activity - funds retained in share.

 - ***transferInOut***: A list of shares eligible for transferring certificate balance into or out of shares and loans as a source of funds for increasing certificate balance 
	 - 1-**shareLoanId**: 4-character share or loan ID
     - 2-**shareLoanName**: share nickname if available otherwise the share description
	 - 3-**shareLoanBal**: the current share available balance

 - ***payeeAddress***:  Comma-delimited member payee address lines representing the address the check will be mailed to should the member elect the 'Disburse funds by check' option. This is a  system calculated value containing from 1 to 6 Payee lines. "null" indicates an invalid address.

 - ***payeeTerms***:  CU customizable terms to be displayed should the member elect the 'Disburse funds by check' option. Optional. May contain from 0 to 9 lines.

 - ***suspendMessage***:  CU customizable message to be displayed should the member select the 'Suspend' option. May contain from 0 to 9 lines.
 
 - ***reviewMessage***:  CU customizable message to be displayed on the review screen. Required. Must contain from 1 to 9 lines.

## Request (PROCESSDATA):
```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.CD.RENEW.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789S0123"},
    {"id": 2, "value": "123456.78"}
  ],
  "userNumList": [
    {"id": 1, "value": 1}
  ],
  "rgSession": 1
}
```
 - userCharList[1]: 10-digit account number, 'S' or 'L' and 4-digit share/Loan ID
	 - Represents the source or target share/loan of the transfer
 - userCharList[2]: User selected option
	 - The transfer in/out amount
 - userNumList[1]: User selected option
	 - Represents the CD renewal option the member selected.

## Successful Response:
```json
{
  "results": "success",
  "updatedState": {
    "name": "My Wonderful CD",
    "maturityDate": "01/01/21",
    "minimumBalance": "12345.67",
    "currentStatus": "Transfer",
    "currentTerm": "24 months",
    "transferSLId": "1234567890S0010",
    "transferAmount": "12345.67"
  }
}
```
 - ***updatedState***
  	 - 1-**name**: Share nickname of targeted certificate/club share. If no nickname, share description.
  	 - 2-**maturityDate**: The current maturity date of the targeted certificate/club.
  	 - 3-**minimumBalance**: The current minimum balance requirement of targeted certificate/club.
  	 - 4-**currentStatus**: The new maturity option set for the targeted certificate/club.
  	 - 5-**currentTerm**: The new term of the targeted certificate/club.
  	 - 6-**transferSLId**: If the current maturity option includes transferring funds into or out of the CD, this will be the current 10-digit account number and share/loan ID.
  	 - 7-**transferAmount**: If the current maturity option includes transferring funds into or out of the CD, this will be the current amount to be transferred.

## Unsuccessful Response:
If any request is not successful for any number of reasons, or the desired action results in an error condition, the PowerOn will respond with the following error structure:

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
