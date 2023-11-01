# Withdraw by check JSON contract

_program version 1.2.0 - 10/24/23_

## STATESTART state

### UX PowerOn Input

```json
{
  "rgState": "STATESTART",
  "powerOnFileName": "BANNO.CHECK.WITHDRAW.V1.POW",
  "userChrList": [
    { "id": 1, "value": "0123456789S0123" },
    { "id": 2, "value": "" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**UX Input Detail**

- userChrList[1]: Account and share or loan ID (13 or 15 character string)
  - 10-digit member number
  - 'S' for Share or 'L' for Loan
  - 2 or 4 digit Share or Loan ID
- userChrList[2-5]: unused
- userNumList[1-5]: unused

### PowerOn Response

**Successful response:**

```json
{
  "results": {
    "eligible": true,
    "memberAccountNumber": "0123456789S0123",
    "shareLoanDescription": "ADVANCED CHECKING",
    "available": "123456.00",
    "minWdAmount": "123456.00",
    "maxWdAmount": "123456.00",
    "owner": "Julie Jones",
    "address": [
      "Julie Jones",
      "6525 Chancellor Drive",
      "Cedar Falls",
      "IA",
      "50613",
      "ADDRESS 6"
    ],
    "disclaimerText": ["disclaimer ", "lines ", "with ", "spaces."]
  }
}
```

**Results detail:**
Unless otherwise noted, all values will be passed as double-quote encapsulated character values

- eligible: Is the member eligible for this service (boolean).
- memberAccountNumber: The 10-digit account number, 'S' or 'L' for Share or Loan, Share or Loan ID
- shareLoanDescription: Share or Loan nickname if available else Share or Loan description
- available:
  - For shares, the available balance
  - For loans, if loan code=3 and loan interest type=10-1899 then loan available cash advance else loan available credit
- minWdAmount: the minimum WD amount (from parameter settings)
- maxWdAmount: the maximum WD amount (from parameter settings)
- owner: The primary member's long name (NAME:LONGNAME)
- address: The system calculated mailing address (ACCOUNT:PAYEELINE[1-6])
- disclaimerText: Custom terms and conditions as set up in the parameter settings Letter file.

**When error condition is encountered:**

```json
{
  "errorCode": 999,
  "loggingErrorMessage": "error message detail"
  "displayErrorMessage": ["display error message detail"]
}
```

**Error detail:**
Unless otherwise noted, all values will be passed as double-quote encapsulated character values

- errorCode: error code generated (numeric)
- loggingErrorMessage: Error message specific to the error code
- displayErrorMessage: An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message.

**Error Code detail:**
|Error Code| Logging Error Message Detail|
|-|-|
|500|Error Opening Letterfile BANNO.CHECK.WITHDRAW.V1.CFG: _[system generated error message]_
|501|Target Share/Loan available balance<=$0.00
|502|Invalid address|
|503|Account not found|
|503|Account warning found|
|503|Target Share/Loan not found|
|503|Share/Loan invalid type|
|503|Share/Loan warning found|
|505|Cross Account access attempt|

## PERFORMWITHDRAW state

### UX PowerOn Input

```json
{
  "rgState": "PERFORMWITHDRAW",
  "powerOnFileName": "BANNO.WITHDRAW.CHECK.V1.POW",
  "userChrList": [
    { "id": 1, "value": "0123456789S0123" },
    { "id": 2, "value": "2.00" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**UX Input Detail**

- userChrList[1]: Account and share or loan ID (13 or 15 character string)
  - 10-digit member number
  - 'S' for Share or 'L' for Loan
  - 2 or 4 digit Share or Loan ID
- userChrList[2]: Dollar amount of withdrawal
- userChrList[3-5]: unused
- userNumList[1-5]: unused

### PowerOn Response

**Successful response:**

```json
{
  "results": {
    "success": true,
    "memoMode": false
  }
}
```

**Results detail:**

- success: Was the attempt successful? (boolean)
- memoMode: Is the system in MemoMode? (boolean)

**When error condition is encountered:**

```json
{
  "errorCode": 999,
  "loggingErrorMessage": "error message detail",
  "requested": "123456.00",
  "minWdAmount": "123456.00",
  "maxWdAmount": "123456.00"
}
```

**Error detail:**

- errorCode: error code generated (numeric)
- loggingErrorMessage: Error message specific to the error code
- displayErrorMessage: An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message.
- requested: The amount of the WD that was requested *--for Error Code 506 only-*
- minWdAmount: the minimum WD amount (from parameter settings) *--for Error Code 506 only--*
- maxWdAmount: the maximum WD amount (from parameter settings) *--for Error Code 506 only--*

**Error Code detail:**
|Error Code| Logging Error Message Detail|
|-|-|
|500|Error processing BANNO.CHECK.WITHDRAW.V1.CFG: _[system generated error message]_
|501|Target Share/Loan available balance<=$0.00
|502|Invalid address|
|503|Account not found|
|503|Account warning found|
|503|Target Share/Loan not found|
|503|Share/Loan invalid type|
|503|Share/Loan warning found|
|504|Reg D error|
|505|Cross Account access attempt|
|506|Amount requested out of bounds|
|507|Amount requested exceeds available/NSF
|508|Unhandled Error (catch-all)|
|509|TRANPERFORM Other Error|

## Additional Information

- The UX input values "userChrList[1-5]" and 'userNumList[1-5] are referenced by the PowerOn as @RGUSERCHAR[1-5]" and @RGUSERNUM[1-5]. Eg: TARGETACCOUNT=@RGUSERCHR1.
- Additional JSON is included in the PowerOn output as debug and program/system information.
  - The additional debug information will only be included for the first 90 days after the latest program version date.
