# CD Renew JSON contract

## Initial Request (PRELOADDATA):

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.CD.RENEW.V1.POW",
  "userChrList": [{ "id": 1, "value": "0123456789S0123" }],
  "userNumList": [],
  "rgSession": 1
}
```

- userCharList[1]: 10-digit account number, 'S' and 4-digit share ID
  - Represents the certificate or club share being updated by the member

## Successful Response:

```json
{
  "results": {
    "currentState": {
      "name": "My Wonderful CD",
      "maturityDate": "01/01/21",
      "minimumBalance": "12345.67",
      "currentStatus": "Renew",
      "renewName": "24 MONTH CERTIFICATE",
      "renewTermPeriod": 24,
      "renewTermFrequency": "Months",
      "ineligibleIrsCode": false,
      "transfers": [
        {
          "transferSLId": "0000800005S0002",
          "transferName": "REGULAR CHECKING",
          "transferAmount": "null",
          "transferPercent": "50.000"
        },
        {
          "transferSLId": "0000800005S0010",
          "transferName": "SUMMER SAVER",
          "transferAmount": "500.00",
          "transferPercent": "null"
        },
        {
          "transferSLId": "0000800005S0000",
          "transferName": "REGULAR SHARE",
          "transferAmount": "0.00",
          "transferPercent": "null"
        }
      ],
      "autoTransfers": [
        {
          "transferSLId": "0000800005S0002",
          "transferName": "REGULAR CHECKING",
          "transferAmount": "600.00"
        }
      ]
    },
    "options": [
      {
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
    "transferFrom": [
      {
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
    "transferTo": [
      {
        "shareLoanId": "1234567890S0000",
        "shareLoanName": "MY PRIME SHARE",
        "shareLoanBal": "1234.56"
      },
      {
        "shareLoanId": "1234567890S0001",
        "shareLoanName": "MY SECONDARY SAVINGS",
        "shareLoanBal": "20000.00"
      },
      {
        "shareLoanId": "1234567890S0030",
        "shareLoanName": "MY 30 MONTH CERTIFICATE",
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
    ],
    "ineligibleIrsCodeMessage": [
      "ineligibleLine1",
      "ineligibleLine2",
      "ineligibleLine3",
      "ineligibleLine4",
      "ineligibleLine5",
      "ineligibleLine6"
    ],
    "ineligibleCrossAccountMessage": [
      "You must be an account owner in order to change maturity options."
    ]
  }
}
```

- **_currentState:_**

  - 1-**name**: Share nickname of targeted certificate/club share. If no nickname, share description.
  - 2-**maturityDate**: The current maturity date of the targeted certificate/club.
  - 3-**minimumBalance**: The current minimum balance requirement of targeted certificate/club.
  - 4-**currentStatus**: The current maturity option set for the targeted certificate/club.
  - 5-**renewName**: New share name/description for a certificate/club renewal.
  - 6-**renewTermPeriod**: The new term period (number of days or months) for the certificate/club renewal.
  - 7-**renewTermFrequency**: The new term frequency (days or months) for the certificate/club renewal.
  - 8-**transfers**: List of existing maturity tranfers.  Funds will be transferred to these shares/loans at maturity.
    - 1-**transferSLID**: If the current maturity option includes transferring funds out of the CD, this will be the current 10-digit account number and share/loan ID.
    - 2-**transferName**: If the current maturity option includes transferring funds out of the CD, this will be the current name of the share/loan.
    - 3-**transferAmount**: If the current maturity option includes transferring funds out of the CD, this will be the current amount to be transferred.
    - 4-**transferPercent**: If the current maturity option includes transferring funds out of the CD, this will be the current percent to be transferred.
  - 9-**autoTransfers**: List of existing auto tranfers.  If the current maturity option includes transferring into the CD, funds will be transferred from these shares at maturity.
    - 1-**transferSLID**: The current 10-digit account number and share ID of the from share.
    - 2-**transferName**: The current name of the from share.
    - 3-**transferAmount**: The current transfer amount.
  - 10-**ineligibleIrsCode**: Set to true if the associated share IRS Code is ineligible for renewal.

- **_options_**: comma-delimited list of available options to member (1 thru 6). Options listed will be based upon the allowable options configured by the CU.

  - 1-**Increase certificate balance**: Add additional funds to the certificate and renew.
  - 2-**Change certificate term**: Roll funds over into a new certificate type (requires CU contact).
  - 3-**Transfer certificate balance**:
    - Full amount - all funds will be transferred out and certificate closed
    - Partial amount - limited to min balance requirement. Certificate will be renewed
  - 4-**Renew**: Renew certificate to current terms.
  - 5-**Disburse funds by check**: Withdraw balance by check, close certificate.
  - 6-**Suspend**: Suspend certificate activity - funds retained in share.

- **_transferFrom_**: A list of shares eligible as a source for transferring funds into the certificate upon maturity (transfer occurs the day before maturity.)

  - 1-**shareLoanId**: 4-character share or loan ID
  - 2-**shareLoanName**: share nickname if available otherwise the share description
  - 3-**shareLoanBal**: the current share available balance

- **_transferTo_**: A list of shares eligible for transferring certificate balance into upon maturity.

  - 1-**shareLoanId**: 4-character share or loan ID
  - 2-**shareLoanName**: share nickname if available otherwise the share description
  - 3-**shareLoanBal**: the current share available balance

- **_payeeAddress_**: Comma-delimited member payee address lines representing the address the check will be mailed to should the member elect the 'Disburse funds by check' option. This is a system
  calculated value containing from 1 to 6 Payee lines. "null" indicates an invalid address.

- **_payeeTerms_**: CU customizable terms to be displayed should the member elect the 'Disburse funds by check' option. Optional. May contain from 0 to 999 lines.

- **_suspendMessage_**: CU customizable message to be displayed should the member select the 'Suspend' option. May contain from 0 to 999 lines.

- **_reviewMessage_**: CU customizable message to be displayed on the review screen. Required. Must contain from 1 to 999 lines.

- **_ineligibleIrsCodeMessage_**: CU customizable message to be displayed for a cd renew request on a share that has an ineligible IRS code. Optional. May contain from 0 to 999 lines.

- **_ineligibleCrossAccountMessage_**: CU customizable message to be displayed to the member when attempting to make CD maturity changes to a cross account.. Optional. May contain from 0 to 999 lines.

## Request (PROCESSDATA):

```json
{
  "rgState": "PROCESSDATA",
  "powerOnFileName": "BANNO.CD.RENEW.V1.POW",
  "userChrList": [
    { "id": 1, "value": "0123456789S0123" },
    { "id": 2, "value": "0123456789S0123" },
    { "id": 3, "value": "123456.78" }
  ],
  "userNumList": [{ "id": 1, "value": 1 }],
  "rgSession": 1
}
```

- userCharList[1]: 10-digit account number, 'S' or 'L' and 4-digit share/Loan ID
  - Represents the certificate or club share being updated by the member
- userCharList[2]: 10-digit account number, 'S' or 'L' and 4-digit share/Loan ID
  - Represents the source or target share/loan of the transfer
- userCharList[3]: User selected option
  - The transfer out amount
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
    "renewName": "24 MONTH CERTIFICATE",
    "renewTermPeriod": 24,
    "renewTermFrequency": "Months",
    "transferSLId": "1234567890S0010",
    "transferAmount": "12345.67"
  }
}
```

- **_updatedState_**
  - 1-**name**: Share nickname of targeted certificate/club share. If no nickname, share description.
  - 2-**maturityDate**: The current maturity date of the targeted certificate/club.
  - 3-**minimumBalance**: The current minimum balance requirement of targeted certificate/club.
  - 4-**currentStatus**: The new maturity option set for the targeted certificate/club.
  - 5-**renewName**: New share name/description for a certificate/club renewal.
  - 6-**renewTermPeriod**: The new term period (number of days or months) for the certificate/club renewal.
  - 7-**renewTermFrequency**: The new term frequency (days or months) for the certificate/club renewal.
  - 8-**transferSLId**: If the current maturity option includes transferring funds into or out of the CD, this will be the current 10-digit account number and share/loan ID.
  - 9-**transferAmount**: If the current maturity option includes transferring funds into or out of the CD, this will be the current amount to be transferred.

## Unsuccessful Response:

If any request is not successful for any number of reasons, or the desired action results in an error condition, the PowerOn will respond with the following error structure:

```json
{
  "errorCode": "XXX",
  "loggingErrorMessage": "[error code detail]"
} // used for Error Codes 500-502, 504 & 506
```

```json
{ // used for Error Codes 503 & 505
    "errorCode": "503",
    "loggingErrorMessage": "[error code detail]",
    "results": {
        "currentState": {
            "name": "My Wonderful CD",
            "maturityDate": "01/01/21",
            "minimumBalance": "12345.67",
            "currentStatus": "Renew",
            "renewName": "24 MONTH CERTIFICATE",
            "renewTermPeriod": 24,
            "renewTermFrequency": "Months",
            "ineligibleIrsCode": false,
                {
                    "transferSLId": "0000800005S0002",
                    "transferName": "REGULAR CHECKING",
                    "transferAmount": "null",
                    "transferPercent": "50.000"
                }
            ],
            "autoTransfers": [
                {
                    "transferSLId": "0000800005S0002",
                    "transferName": "REGULAR CHECKING",
                    "transferAmount": "600.00"
                }
            ]
        }
    }
}
```
## Error Codes

| errorCode | loggingErrorMessage                                             |
| --------- | --------------------------------------------------------------- |
| 500       | Generic Error                                                   |
| 501       | Configuration file error (error opening, reading or processing) |
| 502       | Invalid account or share (based on account type/share warning)  |
| 503       | Maturity selection previously made - contact CU                 |
| 504       | Processing Error                                                |
| 505       | Cross account access attempt                                    |
| 506       | Renew options not set for Share type                            |

