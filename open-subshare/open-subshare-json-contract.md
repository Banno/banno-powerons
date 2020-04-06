# Open subshare JSON contract

To get things started, the client will send the initial request.
The first item in the `userCharList` is the `memberAccountId`

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.OPEN.SUBSHARE.V1.POW",
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

To which the poweron should respond with:

```json
{
  "results": {
    "memoMode": false,
    "canOpenSubShare": true,
    "categories": [
      {
        "name": "Certificate of deposit",
        "accounts": [
          {
            "name": "7 Month certificate",
            "term": "7 months",
            "minBalance": "1000.00",
            "interestRate": "2.450%"
          }
        ]
      }
    ]
  }
}
```
