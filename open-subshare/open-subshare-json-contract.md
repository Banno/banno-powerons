# Open subshare JSON contract

NOTES: every state checks memoMode - if memoMode is true, we cannot proceed.

## PRELOADDATA state

### UX Request

```json
{
  "rgState": "PRELOADDATA",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [],
  "userNumList": [],
  "rgSession": 1
}
```

**UX Request Detail**

- userChrList[1-5]: not used
- userNumList[1-5]: not used

### PowerOn Successful Response

Returns member's eligibility and categories of shares, their related share types and associated interest rates the member can select from.

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
                "apy": "1.210", // apy is optional
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
            "interestRates": [{ "rate": "1.250" }]
          }
        ]
      }
    ]
  }
}
```

**PowerOn Successful Response Detail**

- memoMode: Is the system in memo mode? Boolean
- canOpenSubShare: Is the member eligible to open a new sub-Share? Boolean
- results: Response wrapper
- categories: The categories of shares from which the member can select
  - name: category name
  - groupNumber: category group number (from 0 to 19)
  - accountTypes: list of account types which belong to this group
    - shareType: share type
    - name: default share type name
    - term: share term
    - minBalance: minimum opening balance required for this share type
    - interestRates: one or more interest rate values applicable for this share type
      - rate: interest rate
      - minBalance: min balance associated to this interest rate
      - maxBalance: max balance associated to this interest rate

## GETTERMSFUNDING

The UX returns the member's selected share group and share type.

```json
{
  "rgState": "GETTERMSFUNDING",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    { "id": 1, "value": "groupNumber" },
    { "id": 2, "value": "shareType" },
    { "id": 3, "value": "" },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**UX Request Detail**

- userChrList[1]: Member's selected group number (category)
- userChrList[2]: Member's selected share type
- userChrList[3-5]: not used
- userNumList[1-5]: not used

### PowerOn Successful Response

Returns the specific terms (if any) pertaining to the selected share type, the member's options for funding the new share and required funding amount with a list of eligible shares available to fund from

```json
{
  "memoMode": false,
  "results": {
    "categoryTerms": ["array ", "of ", "strings."],
    "fundingOptions": ["electronic", "check", "later"],
    "minimumFundingAmount": "100.00",
    "electronicFundingAccounts": [
      {
        "name": "Share",
        "availableBalance": "1.00",
        "memberAccountNumber": "1234567890S1234"
      }
    ],
    "feeDisclosure": [
      "1-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "2-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "3-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "4-This is test verbiage4. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage.",
      "5-This is test verbiage3. This is test verbiage. This is test verbiage. This is test verbiage. This is test verbiage."
    ],
    "fee": {
      "amount": "20.00",
      "sourceAccounts": [
        {
          "name": "Savings",
          "availableBalance": "1.00",
          "memberAccountNumber": "1234567890S0000"
        },
        {
          "name": "Checking",
          "availableBalance": "100.00",
          "memberAccountNumber": "1234567890S0010"
        }
      ]
    }
  }
}
```

**PowerOn Successful Response Detail**

- memoMode: Is the system in memo mode? Boolean
- results: Response wrapper
  - categoryTerms: Optional terms relevant to the selected category
  - fundingOptions: any combination of the three available types; 'electronic', 'checking' or 'later'
  - minimumFundingAmount: The minimum opening funding amount
  - electronicFundingAccounts: a list of member accounts they can select from to fund the new Share
    - name: Share description
    - availableBalance: the available balance of the share
    - memberAccountNumber: the 10-digit account number, "S" or "L" for share or loan and the Share or Loan ID
  - feeDisclosure: custom fee disclosure
  - fee: fee parameters if a fee is being charged to create the new sub account
    - amount: the fee amount to be charged
    - sourceAccounts: a list of member accounts that can be selected as a fee source
      - name: Share description
      - availableBalance: the available balance of the share
      - memberAccountNumber: the 10-digit account number, "S" for share and the 4-digit Share ID

## NAMEPRELOAD

Here the UX is returning the total user selections to this point - the group, share type, funding share and type of funding with the amount to fund

```
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

**UX Request Detail**

- userChrList[1]: Member's selected group number (category)
- userChrList[2]: Member's selected share type
- userChrList[3]: Type of funding (electronic, check or later)
- userChrList[4]: Funding source (10-digit account,S/L for Share or Loan and the Share or Loan ID)
- userChrList[5]: Funding amount
- userNumList[1-5]: not used

### PowerOn Successful Response

Returns name record information: existing account level names, name types which can be created and existing Share & Loan names which can be copied to the new share.

```json
{
  "memoMode": false,
  "results": {
    "canAddNames": true,
    "maxNames": 1,
    "nameTypes": [
      { "label": "Beneficiary", "code": "04" },
      { "label": "Joint", "code": "01" }
    ],
    "existingNames": [
      {
        "name": "Watson Sadler",
        "type": "Beneficiary"
      }
    ],
    "eligibleCopyNames": [
      {
        "name": "Sara Sadler",
        "type": "Joint",
        "street": "1234 Main Street",
        "local": "Denver CO 80201",
        "nameLoc": "000133"
      },
      {
        "name": "Julie Sadler",
        "type": "Beneficiary",
        "street": "1234 Main Street",
        "local": "Denver CO 80201",
        "nameLoc": "000255"
      }
    ]
  }
}
```

**PowerOn Successful Response Detail**

- memoMode: Is the system in memo mode? Boolean
- results: Response wrapper
  - canAddNames: Can new name records be added to the new Share (boolean)
  - maxNames: How many (at the most) new name records be added. (program restraints limit this to 2 max - regardless as to what may be set in the configuration program)
  - nameTypes: The name types which can be added
    - label: name type description
    - code: name type numerical representation (NAME:TYPE)
  - existingNames: Current account level name records of the following name types: 1,4,6,8,13,14,19,24,25
    - name: The NAME:LONGNAME from the name record
    - type: The name type description
  - eligibleCopyNames; a list, if any, of any existing name records from other Shares or Loans on the account which are eligible to be copied over to the new Share.
    - name: The NAME:LONGNAME from the name record
    - type: The name type description based on the NAME:TYPE
    - street: The street and, if applicable, extra address of the name record
    - local: The combined city, state and zip code
    - nameLoc: The unique locator code attached to the name record (as a string)

## CREATESHARE

All necessary information is passed back from the UX to create and fund the new share, and create or copy name records under the new share.

```json
{
  "rgState": "CREATESHARE",
  "powerOnFileName": "BANNO.NEWSUBCREATE.V1.POW",
  "userChrList": [
    { "id": 1, "value": "0038;0000;100.00^123;425;1256;24578;125899" },
    { "id": 2, "value": "04;Mary;Quite;Contrary;JR;333222111;mary@aol.com" },
    {
      "id": 3,
      "value": "4519 Fairy Castle Way;Dungeon;Emerald;KS;66104;01011981;1;2125551212"
    },
    { "id": 4, "value": "" },
    { "id": 5, "value": "" }
  ],
  "userNumList": [],
  "rgSession": 1
}
```

**UX Request Detail**

- userChrList[1]: 2 sections of detail. Sections are delimited by a caret symbol, data within each section are delimited by a semicolon.
  - Section 1:
    - share type of new share (1 to 4 numeric characters)
    - share ID of funding share - (2 or 4 characters)
    - funding amount (dollar amount with cents - decimal but no commas)
  - Section 2:
    - up to 10 numeric values representing the name record locator codes of the name records the member wishes to copy under the new share. (NAMEPRELOAD state, copyEligibleNames:nameLoc)
- userChrList[2]:New name record #1 - name type, first name, middle name, last name, name suffix, SSN, email address
- userChrList[3]:New name record #1 (cont) - street addr, extra addr, city, state, zip, DOB, phone type, phone
- userChrList[4]:New name record #2 - name type, first name, middle name, last name, name suffix, SSN, email address
- userChrList[35]:New name record #2 (cont) - street addr, extra addr, city, state, zip, DOB, phone type, phone

### PowerOn Successful Response

Returns the results of the attempt. Because it's possible for the share to be created successfully, but the funding and/or name creation to fail, because of the creation of the share, the transaction is considered a success and reported to the member as a success. Any failures, however, are reported to the CU via email.

```json
{
  "memoMode": false,
  "results": {
    "funding": {
      "type": "electronic",
      "amount": "1.00",
      "successfulTransfer": true
    },
    "newShareId": "35",
    "names": [
      {
        "nameType": "Joint",
        "name": "Bob Jones",
        "created": true
      },
      {
        "nameType": "Beneficiary",
        "name": "Fred Flintstone",
        "created": false
      }
    ]
  }
}
```

**PowerOn Successful Response Detail**

- memoMode: Is the system in memo mode? Boolean
- results: Response wrapper
  - funding: How the new Share was funded
    - type: Type of funding
    - amount: Funding amount
    - successfulTransfer: Was the funding transfer successful? (boolean)
  - newShareId: The system assigned ID to the new Share
  - names: array of new name recors either created or copied over to the new Share
    - nameType: Name type description
    - name: Name attached to the new name record
    - created: Was the creation successful? (boolean)

### PowerOn Response - When error condition is encountered:

```json
{
  "errorCode": 500,
  "loggingErrorMessage": "TRANPERFORM Error:+TRANERROR",
  "displayErrorMessage": "There was an error transferring your opening funds"
}
```

## **PowerOn Error Response Detail**

- errorCode: Numeric error code from 500-504 representing the error condition
- loggingErrorMessage: Additional detail relevant to the error code reported
- displayErrorMessage: Optional member facing error code description determined by the CU.

## Error codes

| Code | Description                             |
| ---- | --------------------------------------- |
| 500  | Program running in memo mode            |
| 501  | Config file read error                  |
| 502  | Account warning exists                  |
| 503  | Ineligible account type                 |
| 504  | No eligible shares to be created        |
| 505  | Error reading ToC for group             |
| 506  | No Shares with sufficient funds to xfer |
| 507  | Error reading passed name info          |
| 508  | Next ID calculation error               |
| 509  | Error getting new share rate            |
| 510  | Could not calculate new maturity date   |
| 511  | Error creating new share                |
