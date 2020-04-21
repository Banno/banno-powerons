# Open subshare JSON contract

To get things started, the client will send the initial request.
The first item in the `userCharList` is the `memberAccountId`

NOTE: every state checks memoMode - if memoMode is true, we cannot proceed

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    {"id": 1, "value": ""},
    {"id": 2, "value": ""},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

To which the poweron should respond with:
NOTE: if memoMode true, categories won't exist

```json
{
  "memoMode": false,
  "results": {
    "canOpenSubShare": true,
    "categories": [
      {
        "name": "Certificate of deposit",
        "groupNumber": "??",
        "accountTypes": [
          {
            "shareType": "??",
            "name": "7 Month certificate",
            "term": "7 months",
            "minBalance": "1000.00",
            "interestRate": "2.450%" // optional
          }
        ]
      }
    ]
  }
}
```

After the user has chosen their desired category/group and accountType, the client will send:

```json
{
  "rgState": "GETTERMSFUNDING",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    {"id": 1, "value": "groupNumber"},
    {"id": 2, "value": "shareType"},
    {"id": 3, "value": ""},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

To which the poweron should respond with:

WHERE: fundingOptions is any combination of the three available types
AND: if "electronic" is an option, the `electronicFundingAccunts` contains a list of available shares to fund the new account from.

NOTE: the 'electronic' type should not be present if the member does not have any valid shares to fund with.

```json
{
  "memoMode": false,
  "results": {
    "categoryTerms":["array ", "of ", "strings."],
    "fundingOptions":["electronic", "check", "later"],
    "electronicFundingAccounts":[
      {
        "name":"Share",
        "availableBalance": "1.00",
        "memberAccountNumber": "1234567890S1234",
      }
    ]
  }
}
```

Once the user agrees to the terms and optionally sets up their electronic funding, the client will send:

```json
{
  "rgState": "NAMEPRELOAD",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    {"id": 1, "value": "groupNumber"},
    {"id": 2, "value": "shareType"},
    {"id": 3, "value": "fundingType"},
    {"id": 4, "value": "memberAccountNumber"},
    {"id": 5, "value": "fundingAmount"}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

To which the poweron should respond with:

```json
{
  "memoMode": false,
  "results": {
    "canAddNames": true,
    "maxNames": "3",
    "nameTypes": ["Beneficiary", "Joint"],
    "existingNames": [
      {
        "name": "Watson Sadler",
        "type": "Beneficiary"
      }
    ],
  }
}
```
