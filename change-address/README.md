
# Change of Address
This program (BANNO.CHANGE.ADDR.V1.POW) enables end users to submit an address change to the financial institution (FI). Depending on the set-up, the change will either immediately update the member's address on
the core or start a conversation. The conversation to the FI will include the member's updated address, plus any notes the member added with the expectation the FI staff will then manually update the member's
record in the system. The program allows for submitting changes to the following address fields
* Street
* Extra Address
* City
* State
* Zipcode

The corresponding configuration Letter file (BANNO.CHANGE.ADDR.V1.CFG) gives you the ability to configure the following options. Instructions for completing the Letter file are in the Letter file itself.
* Ineligible account types
	* Members with these account types will require manual intervention for an address change
* Exclusionary account level warning codes
	* Members with these account level warning types will require manual intervention for an address change
* Account warning code to clear upon successful address update
	* This, single, warning code will be cleared upon the successful address update
* Account warning code to set upon successful address update
	* This, single, warning code will be set upon the successful address update
* Restrictive use of post office box addresses
	* Should the program restrict use of a PO Box in the street address field.
* PO Box List
	* If PO Boxes are being restricted, a list of PO Box address formats for the program to validate against, i.e. 'PO Box', 'P.O. Box', 'Post Office Box', etc.
* Name Level Matching
	* If enabled, all allowed name records on the member account that match the old address are updated. Enter list of allowed account level name records.
* Match Name Types
	* If Name Level Matching is enabled, enter a list of allowed name types.
* Test Accounts
	* If not blank, PowerOn will perform the latest changes to the PowerOn only for the accounts listed. Other accounts' experiences will remain unchanged.

A successful address update by the program or program errors encountered while attempting to update the member's address will  result in a note record added to the member's account:


## PowerOn installation
An Overview 
* If using Name Level Matching:
	* Install the program (BANNO.CHANGE.ADDR.V1.POW) to the REPWRITERSPECS directory*
	* Add program to SymXchange Common parameters and take SymXchange off-host, then back on-host
	* Install the configuration Letter file (BANNO.CHANGE.ADDR.V1.CFG) to the LETTERSPECS directory and update the settings as necessary*
	* In Banno People, set the appropriate settings and activate the program.

* If NOT using Name Level Matching:
	* Install the programs (BANNO.CHANGE.ADDR.V1.POW & BANNO.DATABASE.CHECK.V1.POW) to the REPWRITERSPECS directory*
	* Add both programs to SymXchange Common parameters and take SymXchange off-host, then back on-host
	* Install the configuration Letter file (BANNO.CHANGE.ADDR.V1.CFG) to the LETTERSPECS directory and update the settings as necessary*
	* In Banno People, set the appropriate settings and activate the program.

In Detail
*[Click here](https://github.com/Banno/banno-powerons) for step-by-step instructions on installing the PowerOn and configuration Letter file on your host.

You may also open a case, requesting installation of the program for you in which case you'll need to fill out the questionnaire 
(https://github.com/Banno/banno-powerons/blob/master/change-address/change-address-questionnaire.docx) which the Banno team will then use to install the program and set up your
configuration file.
## Required files
* PowerOn Name:  `BANNO.CHANGE.ADDR.V1.POW`
* Configuration Letter file Name:   `BANNO.CHANGE.ADDR.V1.CFG`
* (Only if not using Name Level Matching) Link User to Episys Name Record PowerOn: `BANNO.DATABASE.CHECK.V1.POW`
	* This file can be obtained from the [link-user-to-episys-name-record](https://github.com/Banno/banno-powerons/tree/master/link-user-to-episys-name-record) section of the Banno GitHub repository.

## Supported SymXchange web consoles
* PowerOnService - 2017.01 V1
* PowerOnService - 2018.00
* PowerOnService - 2018.01