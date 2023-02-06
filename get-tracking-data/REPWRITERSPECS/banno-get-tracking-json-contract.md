# Get Tracking Data JSON contract

## GETTRACKINGINFO state

### PowerOn Input

```json
{
  "rgState": "GETTRACKINGINFO",
  "powerOnFileName": "BANNO.GETTRACKING.V1.POW",
  "userChrList":[
    {"id": 1, "value": "0123456789"},
    {"id": 2, "value": "2-15,18,25-68,72-9999"},
    ],
  "userNumList": []
  "rgSession": 1
}
```

- userCharList[1]: Episys member number
- userCharList[2]: Comma delimited list of tracking types to include

## PowerOn Response

```json
{
  "results": [
    {
      "LOCATORNUM": "192",
      "TRACKINGTYPE": "89",
      "USERCHAR01": "Q3 2021",
      "USERCHAR02": "Q4 2021",
      "USERCHAR03": "Q1 2022",
      "USERCHAR04": "Q2 2022",
      "USERCHAR05": "Q3 2022",
      "USERNUMBER01": "800",
      "USERNUMBER02": "750",
      "USERNUMBER03": "730",
      "USERNUMBER04": "770",
      "USERNUMBER05": "799"
    },
    {
      "LOCATORNUM": "117",
      "TRACKINGTYPE": "01",
      "USERAMOUNT01": "567899.99",
      "USERAMOUNT06": "999.99",
      "USERCHAR01": "56789777",
      "USERDATE02": "01/01/1989",
      "USERDATE05": "02/02/1999",
      "USERRATE01": "25.002"
    },
    {
      "LOCATORNUM": "119",
      "TRACKINGTYPE": "35",
      "USERCHAR01": "DKTT8IFB"
    },
    {
      "LOCATORNUM": "127",
      "TRACKINGTYPE": "08",
      "USERCHAR12": "14",
      "USERCHAR13": "8f029c9f-faf5-4d18-a167-9b5a302d9ba5",
      "USERCHAR14": "20170280000000144",
      "USERCHAR15": "20210950000000000",
      "USERNUMBER16": "99",
      "USERNUMBER20": "88"
    },
    {
      "LOCATORNUM": "128",
      "TRACKINGTYPE": "08",
      "USERCHAR12": "14",
      "USERCHAR13": "842ac7ce-e69c-41f1-bd64-672cbdcf3961",
      "USERCHAR14": "20170280000000144",
      "USERCHAR15": "20210950000000000"
    }
  ]
}
```
- results: An array of tracking record objects
	 - LOCATORNUM: The Tracking Locator field in Episys
   - TRACKINGTYPE: The Tracking Type field in Episys
   - USERCHARxx: Tracking User Char fields
   - USERAMOUNTxx: Tracking User Amount fields
   - USERCODExx: Tracking User Code fields
   - USERDATExx: Tracking User Date fields
   - USERNUMBERxx: Tracking User Number fields
   - USERRATExx: Tracking User Rate fields
