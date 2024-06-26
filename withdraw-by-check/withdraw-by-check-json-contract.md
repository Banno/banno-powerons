# Withdraw by check JSON contract

_program version 1.2.1 - 06/07/24_

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
  "loggingErrorMessage": "error message detail",
  "displayErrorMessage": ["display error message detail"]
}
```

**Error detail:**

Unless otherwise noted, all values will be passed as double-quote encapsulated character values

- errorCode: error code generated (numeric)
- loggingErrorMessage: Error message specific to the error code
- displayErrorMessage: An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message.

**Error Code detail:**

| Error Code | Logging Error Message                                                                 | Modifier                                                                             | Additional Notes As Needed                    |
| ---------- | ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| 500        | Error Opening Letterfile [configuration file name] | : [system generated letter file read error message] ||
|            | Error Reading Letterfile [configuration file name] | : [system generated letter file read error message] ||
|            | CFG Letterfile Error | : No Share/Loan types defined. ||
| 501        | Avail. Balance <= $0.00 || Target Share/Loan available balance<=$0.00 |
| 502        | Invalid Address |||
| 503        | Account Not Found || Error Code 503 is used for account find/validate errors |
|            | Account Warning Found | : [3-digit comma separated account warning list] ||
|            | Share Not Found |||
|            | Loan Not Found |||
|            | Invalid Share Type | : [4-digit share type] ||
|            | Invalid Loan Type | : [4-digit loan type] ||
|            | Share Warning Found | : [3-digit comma separated share warning list] ||
|            | Loan Warning Found | : [3-digit comma separated loan warning list] ||
| 505        | Cross Account WD Attempted |||

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
  "displayErrorMessage": ["display error message detail"],
  "requested": "123456.00",
  "minWdAmount": "123456.00",
  "maxWdAmount": "123456.00"
}
```

**Error detail:**

- errorCode: error code generated (numeric)
- loggingErrorMessage: Error message specific to the error code
- displayErrorMessage: An array of up to 5 display lines. If included, this message will display in place of the hard-coded UX display message.
- requested: The amount of the WD that was requested _--for Error Code 506 only-_
- minWdAmount: the minimum WD amount (from parameter settings) _--for Error Code 506 only--_
- maxWdAmount: the maximum WD amount (from parameter settings) _--for Error Code 506 only--_

**Error Code detail:**

*See the Modifier section for additional details.
 The Modifier is appended to the main Logging Error Message.

| Error Code | Logging Error Message                                                                 | Modifier                                                                             | Additional Notes As Needed                    |
| ---------- | ------------------------------------------------------------------------------------- |------------------------------------------------------------------------------------- |-----------------------------------------------|
| 500        | Error Opening Letterfile [configuration file name] | : [system generated letter file read error message] ||
|            | Error Reading Letterfile [configuration file name] | : [system generated letter file read error message] ||
|            | CFG Letterfile Error | : No Share/Loan types defined. ||
| 501        | Avail. Balance <= $0.00 || Target Share/Loan available balance<=$0.00 |
| 502        | Invalid Address |||
| 503        | Account Not Found || Error Code 503 is used for account find/validate errors |
|            | Account Warning Found | : [3-digit comma separated account warning list] ||
|            | Share Not Found |||
|            | Loan Not Found |||
|            | Invalid Share Type | : [4-digit share type] ||
|            | Invalid Loan Type | : [4-digit loan type] ||
|            | Share Warning Found | : [3-digit comma separated share warning list] ||
|            | Loan Warning Found | : [3-digit comma separated loan warning list] ||
| 504        | TRANPERFORM Error | : [transaction posting system error message. contains phrase "REG D"] | Reg D Limit |
| 505        | Cross Account WD Attempted |||
| 506        | Amount requested out of bounds |||
| 507        | Amount req.  ###,##9.99  exceeds avail.  ###,##9.99 |||
|            | TRANPERFORM Error | : NSF | Insufficient Funds |
| 508        | Unhandled Error || (catch-all) |
| 509        | TRANPERFORM Error | : [transaction posting system error message] | other transaction posting system error |

## Additional Information

- The UX input values userChrList[1-5] and userNumList[1-5] are referenced by the PowerOn as @RGUSERCHAR[1-5] and @RGUSERNUM[1-5]. Eg: TARGETACCOUNT=@RGUSERCHR1.
- Additional JSON is included in the PowerOn output as debug and program/system information.
  - The additional debug information will only be included for the first 90 days after the latest program version date.
