# Open subshare JSON contract

NOTES: every state checks memoMode - if memoMode is true, we cannot proceed.

## PRELOADDATA
To get things started, the client will send the initial PRELOADDATA request.
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

Poweron response:

```json
{
  "memoMode": false,
  "canOpenSubShare": true,
  "results": {
    "categories": [
      {
        "name": "Certificate of deposit",
        "groupNumber": "0",
        "accountTypes": [
          {
            "shareType": "0",
            "name": "7 Month certificate",
            "term": "7 months",
            "minBalance": "1000.00",
            "interestRates": [
              {
                "rate": "0.000",
                "minBalance": "0.00",
                "maxBalance": "4.99"
              },
              {
                "rate": "1.200",
                "minBalance": "5.00",
                "maxBalance": "99.99"
              }
            ]
          },
          {
            "shareType": "1",
            "name": "7 Month certificate",
            "term": "7 months",
            "minBalance": "1000.00",
            "interestRates": [{"rate":"1.250"}]
          }
        ]
      }
    ]
  }
}
```
### Errors
```json
[500]
```

## GETTERMSFUNDING
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

Poweron response:

* fundingOptions is any combination of the three available types
* if "electronic" is an option, the `electronicFundingAccunts` contains a list of available shares to fund the new account from.
* the 'electronic' type should not be present if the member does not have any valid shares to fund with.
* categoryTerms may be empty
```json
{
  "memoMode": false,
  "results": {
    "categoryTerms": ["array ", "of ", "strings."],
    "fundingOptions": ["electronic", "check", "later"],
    "minimumFundingAmount": "100.00",
    "electronicFundingAccounts": [
      {
        "name":"Share",
        "availableBalance": "1.00",
        "memberAccountNumber": "1234567890S1234",
      }
    ]
  }
}
```
### Errors
```json
[500, 503, 501]
```

## NAMEPRELOAD
Once the user agrees to the terms and optionally sets up their electronic funding, the client will send:

```json
{
  "rgState": "NAMEPRELOAD",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    {"id": 1, "value": "groupNumber"},
    {"id": 2, "value": "shareType"},
    {"id": 3, "value": "fundingType"},
    {"id": 4, "value": "fundingMemberAccountNumber"},
    {"id": 5, "value": "fundingAmount"}
  ],
  "userNumList": [],
  "rgSession": 1
}
```

Poweron response:

```json
{
  "memoMode": false,
  "results": {
    "canAddNames": true,
    "maxNames": 1,
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
### Errors
```json
[500, 501]
```

## Adding names
The client will send a max of 2 names using userCharList items 2-5, where 2-3 are the first, and 4-5 are the second.
item 1 contains the new share info:

newShareType;transferFromId;transferAmount

items 2 and 4:

nameType;first;middle;last;suffix;ssn;email

item 3 and 5:

street;address2;city;state;zip;dob;phoneType;phoneNumber

#### max characters:
| Count  | Description         |
|--------|---------------------|
| 4      | newShareType        |
| 4      | transferFromId      |
| 15     | transferAmount      |
| 2      | nameType*           |
| 20     | first               |
| 10     | middle              |
| 40     | last                |
| 4      | suffix              |
| 9      | ssn                 |
| 40     | email               |
| 40     | street              |
| 20     | address2            |
| 31     | city                |
| 2      | state               |
| 5      | zip                 |
| 8      | dob                 |
| 1      | phoneType           |
| 10     | phoneNumber         |

* nameType is the numeric index from the nameTypes returned via the NAMEPRELOAD request

```json
{
  "rgState": "ADDNAMES",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    {"id": 1, "value": "0038;0000;100.00"},
    {"id": 2, "value": "04;Mary;Quite;Contrary;JR;333222111;mary@aol.com"},
    {"id": 3, "value": "4519 Fairy Castle Way;Dungeon;Emerald;KS;66104;01011981;1;2125551212"},
    {"id": 4, "value": ""},
    {"id": 5, "value": ""}
  ],
  "userNumList": [],
  "rgSession": 1
}
```


## Error Information
If a request is not successful for any number of reasons, the poweron should respond with the following structure:

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "TRANPERFORM Error:+TRANERROR"
}
```

## Error codes
| Code   | Description         |
|--------|---------------------|
| 500    | Generic Error       |
| 501    | Try again           |
| 502    | Missing address     |
| 503    | Invalid loan/share  |
| 504    | Reg D Limit         |
