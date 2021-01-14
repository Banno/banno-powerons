#JSON contract for BANNO.M2MTRANSFERS.V1.POW

## PRELOADDATA

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890" }, // 10-digit account
    { "id": 2, "value": "" }, // userchar[2-5] is unused
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [], //  usernum[1-5] is unused
  "rgSession": 1
}
```

PowerOn response - list of eligible shares, list of scheduled transfers, list of saved recipients

```json
{
  "transfers": [
    {
      "transferSLId": "0000800005S0002",
      "transferName": "SUMMER SAVER",
      "transferAmount": "null",
      "transferPercent": "50.000"
    },
    {
      "transferSLId": "0000800005S0010",
      "transferName": "REGULAR CHECKING",
      "transferAmount": "500.00",
      "transferPercent": "null"
    },
    {
      "transferSLId": "0000800005S0015",
      "transferName": "ULTIMATE CHECKING (10)",
      "transferAmount": "0.00",
      "transferPercent": "null"
    }
  ],
  "scheduledTransfers": [
    {
      "shareId": "1234567890L0020",
      "accountName": "CHECKING",
      "transferAmt": "120.00",
      "recipientName": "Emily Fitch",
      "recipientMemberId": "9876543210",
      "recipientShareId": "9876543210L0001",
      "recipientNickname": "Emmy",
      "transferDate": "08/31/2020",
      "transferFrequency": "weekly"
    },
    {
      "shareId": "1234567890L0001",
      "accountName": "SAVINGS",
      "transferAmt": "120.00",
      "recipientName": "Howard Cleo",
      "recipientMemberId": "9876543210",
      "recipientShareId": "9876543210L0001",
      "recipientNickname": "Howie",
      "transferDate": "12/31/2025",
      "transferFrequency": "once"
    }
  ],
  "savedRedipients": [
    {
      "recipientName": "Binsy Flomor",
      "recipientMemberId": "9876543210",
      "recipientShareId": "9876543210L0001",
      "recipientNickname": "Binsy"
    },
    {
      "recipientName": "Kylie Crenshaw",
      "recipientMemberId": "9876543210",
      "recipientShareId": "9876543210L0001",
      "recipientNickname": "KC"
    }
  ]
}
```

Poweron response - Configuration File read error:

```json
{
  "errorCode": "501",
  "loggingErrorMessage": "Error reading from config file"
}
```

PowerOn response - Invalid Account (by Account Warning):

```json
{
  "errorCode": "502",
  "loggingErrorMessage": "Invalid Account - account warning xxx"
}
```

PowerOn response - If shares(s) not found:

```json
{
  "errorCode": "503",
  "loggingErrorMessage": "No eligible shares found"
}
```

## AUTHORIZEPAYMENT, PERFORMPAYMENT

Because the Specfile functionality is basically the same in both of these states,
the same returns will apply to each state. The state name will indicate whether
the transaction will be performed as a pre-auth or as a final transaction.

```json
{
  "rgState": "[AUTHORIZEPAYMENT,PERFORMPAYMENT]",
  "powerOnFileName": "BANNO.M2MTRANSFERS.V1.POW",
  "userChrList": [
    { "id": 1, "value": "1234567890L0010" }, // 10-digit account+'L'+LOAN:ID
    { "id": 2, "value": "1234567.89" }, // payment amount
    { "id": 3, "value": "" }, // userchar3-5 is unused
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [], //  usernum1-5 is unused
  "rgSession": 1
}
```

Poweron response - successful / unsuccessful:

```json
{
  "results": {
    "memoMode": [true, false],
    "memberTransfer": {
      "shareId": "1234567890L0001",
      "accountName": "SAVINGS",
      "transferAmt": "120.00",
      "recipientName": "Howard Cleo",
      "recipientMemberId": "9876543210",
      "recipientShareId": "9876543210L0001",
      "recipientNickname": "Howie",
      "transferDate": "12/31/2025",
      "transferFrequency": "once"
    }
  }
}
```

Error Codes
501 - Config file read / validation error
502 - Invalid Account
