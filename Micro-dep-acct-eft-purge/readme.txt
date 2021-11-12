PowerOn Name:       BANNO.MICRO.BATCH.FM.V1.POW
Copyright 2021 Jack Henry and Associates


When a CU is using Banno FI to FI transfers, a small amount is sent to the new external
account to verifiy it is a valid account. This amount is sent as an ACH origination from
a specfied micro-deposit account. On this account, the expired EFT records get too 
numerous causing slowness in that account and with reports that review the account. Banno
suggests this PowerOn be run periodically to purge these EFT record as they are no longer
useful. This PowerOn is used in Episys via Batch Control. It will produce a report of 
EFT records to be used for mass deletion of the records.


This PowerOn is run from batch control, PowerOn generator. PowerOn prompts for the number of 
days to keep the expired records and the account number used for the micro deposits. Output
of PowerOn should be reviewed in Print Control to verify the account number entered was
valid. Once run, the CU must run Miscellaneous Processing from batch control to post the FM 
to the account. 


Report Output: File maintenance to be read by miscellaneous processing. Posting journal to
               review after posting.


For more information check
https://github.com/Banno/banno-powerons

Banno is not responsible for any modifications to this file
made by unauthorized personnel.