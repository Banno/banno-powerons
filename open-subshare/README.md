# Open SubShare
The Open Subshare program allows the end-user to create and fund a new sub-share. The end-user may also elect to add up to two new account or share level name records (based upon parameter settings.

Program configuration is accomplished through the use of an on-demand program BANNO.NEWSUBCREATE.V1.CONFIG.

By running the configuration program from the Episys/Quest account manager or teller transaction work spaces, the following configuration options can be set.
* Account warnings to exclude
* Account type to exclude
* Should funding the new sub-share be required
* should there be a minimum funding requirement (regardles of opening/min. balance requirements
* Can name records be added and if so, how many and at account or share level.
* What are 'safe' name types (those whcih can be immediately added)
* What are 'unsafe' name types (those which will need to be manually added by the CU
* What share tracking type should be used to store unsafe name records to be added until manually added by the CU
* Should the share dividend rates be displayed
* Should the share div rate be filled in at the new share creation

In addition, the configuration program allows the credit union to extablish up to 10 share groups the end-user may select from and which share types will be offered to the member under each group. For each share type, the credit union can then set the new share ID range the program will draw from when the new share is created.

## PowerOn installation
[Click here](https://github.com/Banno/banno-powerons) to learn more about how to install the PowerOn on your host. 

## Required files
* PowerOn Name:  `BANNO.NEWSUBCREATE.V1.POW`
* Configuration Program Name:   `BANNO.NEWSUBCREATE.V1.CONFIG`

## Supported SymXchange web consoles
* PowerOnService - 2017.01 V1
* PowerOnService - 2018.00
* PowerOnService - 2018.01

_In order for your PowerOn to show-up you'll need to contact your support or implementation leader to turn on the Loan Payoff linktype._

