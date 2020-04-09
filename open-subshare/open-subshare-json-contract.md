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
    "memoMode": false,
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

to which this poweron should respond with:

```json
{
  "memoMode":false,
  "results": {
    "categoryTerms":["array ", "of ", "strings."],
    "fundingOptions":[],
  }
}
```

Once the user agrees to the terms, the client will send:

```json
{
  "rgState": "NAMEPRELOAD",
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

```json
{
  "memoMode": false,
  "results": {
    "canAddNames": true,
    "existingNames": [],
  }
}
```
