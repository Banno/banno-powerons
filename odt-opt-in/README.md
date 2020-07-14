#ODT Opt In PowerOn

##This service PowerOn allows members to Opt-in or Opt-out for the Reg E ODT service.

Members are presented with a list of eligible shares whereby they may chose to opt-in or opt-out on an individual share-by-share basis. Eligible shares are those shares which meet the settings as defined in the parameter configuration Letter file (BANNO.ODTOPTIN.V1.CFG). The configuration Letter file allows the credit union to establish:
  * Ineligible account type(s)
  * Eligible Share Types.
  * Disqualifying account level warning(s).
  * Disqualifying share level warning(s).
  
Additional parameter Letter file settings allow the credit union to:
  * Set the share tracking record type the program should use for saving member choices.
  * Determine whether the program should also update the share's ODTAUTHFEESRCCODELIST:1 and AUTHFEEOPTION:1 fields.
  * Establish what the ODTAUTHFEESRCCODELIST:1 settings will be for when the member opts in and opts out.
  * Establish what the AUTHFEEOPTION:1 settings will be for when the member opts in and opts out.
  * Set the message verbiages for the terms and conditions, fee disclosure and revocation instruction messages.
  
Upon successful submission of the member's selections the program:
  1. Creates a share tracking record (or updates the existing one) with the member's selection and date.
  2. Updates the share's ODTAUTHFEESRCCODELIST:1 & AUTHFEEOPTION:1 fields if set in the parameter Letter file
  
## Program limitations
The program can display and process up to 130 eligible shares on the member account. Should the member have more eligible shares, they will be presented with a message indicating that they will need to contact the credit union to update the additional shares.

## JSON Contract  
  [JSON contract](https://github.com/Banno/banno-powerons/blob/master/odt-opt-in/ODT%20Opt-in%20JSON%20Contract.md)
  