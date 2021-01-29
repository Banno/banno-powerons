# Open a Subshare
The Open a Subshare PowerOn allows the end-user to open a subshares whenever it’s convenient for them. While creating their new subshare, the members can easily add additional names associated with the subshare and add funds to their new account via transfer. 

Account types included (but not limited to): 
* Certificate of deposit
* Checking
* Club
* Savings 
* Money market

Included in the PowerOn files is a configuration tool (BANNO.NEWSUBCREATE.V1.CONFIG) which is an on-demand PowerOn, to be run from the Account Manager, which helps you customize the PowerOn to your needs. The tool will generate a configuration datafile used by the program.

## Required files
* PowerOn Name: BANNO.NEWSUBCREATE.V1.POW
* Configuration PowerOn: BANNO.NEWSUBCREATE.V1.CONFIG

## Setup Steps
In addition to the normal setup process for the PowerOn program, 
* Run the configuration program (BANNO.NEWSUBCREATE.V1.CONFIG) from the Account Manager. The program contains a built-in help file explaining each setting and how it can be used.
	* The configuration program saves your desired options to a datafile which is used by the PowerOn program at run-time
	* The configuration program also creates, as necessary, Terms or Conditions Letterfiles which can be updated as needed for each of the share groups you're making available for members to select from.

## Supported SymXchange web consoles:
* PowerOnService - 2017.01 V1
* PowerOnService - 2018.00
* PowerOnService - 2018.01

*In order for your PowerOn to display to your customers, you'll need to contact your support or implementation leader to turn on the Open a Subshare linktype.*
