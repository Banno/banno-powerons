#JSON contract for BANNO.UNVERXFERS.V1.POW

## PRELOADDATA STATE
### Client Request (PRELOADDATA)

```jsonc
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.UNVERXFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "" }, // userchar[1-5] is unused
    { "id": 2, "value": "" }, 
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [], //  usernum[1-5] is unused
  "rgSession": 1
}
```

### PowerOn Successful Response (PRELOADDATA)
List of available loans, scheduled EFTs and saved external accounts.

```jsonc
{
  "currentState": {
    "availableLoans": [
      {
        "destinationId": "0000800005L0002",
        "name": "NEW AUTO",
        "paymentAmount": "500.00",
        "paymentDue": "504.44",
        "payOff": "12635.22",
        "payOffDate": "04/11/2021" // system date
      },
      {
        "destinationId": "0000800005L0030",
        "name": "LINE OF CREDIT",
        "paymentAmount": "50.00",
        "paymentDue": "50.00",
        "payOff": "502.84",
        "payOffDate": "04/11/2021" // system date
      },
      {
        "destinationId": "0000800005L0060",
        "name": "USED BOAT",
        "paymentAmount": "206.55",
        "paymentDue": "206.55",
        "payOff": "7696.52",
        "payOffDate": "04/11/2021" // system date
      }
    ],
    "scheduledefts": [
      {
        "eftLoc": "215",
        "destinationId": "0000800005L0002",
        "accountName": "NEW AUTO",
        "transferAmt": "500.00",
        "externalFi": "WELLS FARGO",
        "externalFiRouting": "201587184",
        "externalAcctNum": "9876543210",
        "externalAccountType": "savings",
        "startDate": "07/31/2021",
	    "nextTransferDate":"08/31/2021",
        "transferFrequency": "monthly",
        "day1": "31",
        "day2": ""
      },
      {
        "eftLoc": "889",
        "destinationId": "0000800005L0060",
        "accountName": "USED BOAT",
        "transferAmt": "206.55",
        "externalFi": "US BANK",
        "externalFiRouting": "211527458",
        "externalAcctNum": "1234567890",
        "externalAccountType": "checking",
        "startDate": "07/07/2021",
	    "nextTransferDate":"08/07/2021",
        "transferFrequency": "semi-monthly",
        "day1": "7",
        "day2": "21"
      }
    ],
    "savedExternalAccounts": [
      {
        "externalLoc": "5443231543",
        "externalFiName": "WELLS FARGO",
        "externalFiRouting": "201587184",
        "externalAccount": "9876543210",
        "externalAccountType": "savings",
        "externalNickName": "My Wells Fargo"
      },
      {
        "externalLoc": "546433",
        "externalFiName": "US BANK",
        "externalFiRouting": "211527458",
        "externalAccount": "1234567890",
        "externalAccountType": "checking",
        "externalNickName": "My US Bank"
      }
    ]
  }
}
```
### PowerOn Error Responses (PRELOADDATA)
#### PowerOn response - Configuration File read error:
```jsonc
{
  "errorCode": "501",
  "loggingErrorMessage": "Error [opening/reading] from config file: [error msg]"
}
```
### PowerOn response - Invalid Account (by Account Warning):

```jsonc
{
  "errorCode": "502",
  "loggingErrorMessage": "Invalid Account - account warning xxx"
}
```
### PowerOn response - No Eligible Loans(s) found:

```jsonc
{
  "errorCode": "503",
  "loggingErrorMessage": "No eligible loans found"
}
```
## CREATE EFT REQUEST STATE
### Client Request (CREATEEFT)

```jsonc
{
  "rgState": "CREATEEFT",
  "powerOnFileName": "BANNO.UNVERXFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "221380114|123455432|WELLS FARGO|checking|yes|My Wells Fargo" },  // [external routing]|[external account number]|[actual FI Name]|[external account type]|[save FI?]|[FI Nickname]
    { "id": 2, "value": "0000008000L0002|semimonthly|04012021|day1|day2" }, // [destination loan]|[frequency]|[start date]|[day 1]|[day 2]
    { "id": 3, "value": "" },  // 3-5 unused
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
    { "id": 1, "value": 546433 }, // recipientLoc if available
    { "id": 2, "value": 504.44 }  // amount
  ],
  "rgSession": 1
}
```
* CREATEEFT - Create new EFT record.
	* Existing External Account: num1 will send locator, chr1 will be blank
	* New External Account: chr1 will send all needed FI data and num1 will be 0 
	* All External Accounts: chr2 will send all needed frequency data, num2 will send amount

### PowerOn Successful Response (CREATEEFT)
```jsonc
{
  "results": {
    "success": "true",
    "currentState": "[PRELOADDATA]"
  }
}
```

### Client Request (EDITEFT)

```jsonc
{
  "rgState": "EDITEFT",
  "powerOnFileName": "BANNO.UNVERXFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "monthly|12/31/2021|31|0" }, // [transferFrequency]|[startDate]|[day1]|[day2]
    { "id": 2, "value": "" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [
    { "id": 1, "value": 855 },    // eftLoc
    { "id": 2, "value": 1000.51 } // amount
  ],
  "rgSession": 1
}
```
EDITEFT - Edit existing EFT 
	* eftLoc, transferAmt, transferFrequency, startDate, day1, day2

### PowerOn Successful Response (EDITEFT)
```jsonc
{
  "results": {
    "success": true,
    "currentState": "[PRELOADDATA]"
  }
}
```

### Client Request (DELETEXTACCT)

```jsonc
{
  "rgState": "DELETEEXTACCT",
  "powerOnFileName": "BANNO.UNVERXFERS.V1.POW",
  "userChrList": [],   
  "userNumList": [
    {"id": 1, "value": 546433} // external account Loc
  ],  
  "rgSession": 1
}
```

### Client Request (DELETEEFT)

```jsonc
{
  "rgState": "DELETEEFT",
  "powerOnFileName": "BANNO.UNVERXFERS.V1.POW",
  "userChrList": [],
  "userNumList": [
    { "id": 1, "value": 855 } // EFT Loc
  ],  
  "rgSession": 1
}
```

### PowerOn Successful Response (DELETEEFT)
```jsonc
{
  "results": {
    "success": true,
    "currentState": "[PRELOADDATA]"
  }
}
```


### PowerOn Error Response (CREATEEFT,EDITEFT,DELETEEXTACCT,DELETEEFT)
```jsonc
{
  "results": {
    "errorCode": "5xx",
    "loggingErrorMessage": "error code description"
  }
}
```

### Error Codes
500 - System in Memo Mode
501 - Config file read / validation error
502 - Account Warning Found
503 - No Eligible Loans
504 - Insufficient information
505 - Invalid input
506 - Error creating/deleting External account
507 - Error creating/updating/deleting transfer
508 - Undefined error

### Transfer Frequencies
The following strings are all valid transfer frequencies:
once, weekly, monthly, semimonthly, biweekly, quarterly(?), yearly(?)

### Configuration File 
The following items will be read from a configuration letter file:
ACH Group code - Group code for running ACH Origination
Loan types - Loan types that will be allowed
Account warnings - What account warnings makes an account ineligible



