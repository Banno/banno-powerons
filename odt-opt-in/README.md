# Overdraft opt-in/out PowerOn
The Overdraft opt-in/out PowerOn allows the end-user to acknowledge the credit union’s terms and fees and opt-in or out of overdraft services on their account. The member may opt-in or opt-out on a share 
by share basis. The current status of any given share and the date last set will be recorded by the program in a share tracking record.

A configuration Letter file gives the credit union the ability to customize certain operational aspects of the program.
* The share tracking type to be used to store the current ODT status for each given share
	* Any available share tracking record between 30 and 99 may be selected
* Ineligible account types
	* Accounts of this type are ineligible to use the program and the member will be directed to contact the CU
* Eligible share types
	* Only shares of these types can have the ODT status set by the program
* Account level warnings
	* Accounts with any of these warnings are ineligible to use the program and the member will be directed to contact the CU
* Share level warnings
	* Shares with any of these warnings are ineligible to have the ODT updated by the program
* Update ODT Auth/Fee Option 1  and ODT Source Code List 1 fields
	* Should the program update the share's ODT Auth/Fee Option 1 and ODT Source Code List 1 fields at the time the share is updated
* ODT Source Code List 1
	* The value the Share's ODT Source Code List 1 field will be set to if enrolled or unenrolled in ODT and the Update ODT Auth/Fee Option 1 option is set to true
* ODT Auth/Fee Option 1
	* The value the Share's ODT Auth/Fee Option 1 field will be set to if enrolled or unenrolled in ODT and the Update ODT Auth/Fee Option 1 option is set to true
* Customizable customer terms and conditions
* Customizable fee disclosure
* Customizable revocation instructions

## PowerOn installation
[Click here](https://github.com/Banno/banno-powerons) to learn more about how to install the PowerOn on your host.

## Required files
* PowerOn Name:  `BANNO.ODTOPTIN.V1.POW`
* Letter file Name:   `BANNO.ODTOPTIN.V1.CFG`

## Supported SymXchange web consoles
* PowerOnService - 2017.01 V1
* PowerOnService - 2018.00
* PowerOnService - 2018.01
