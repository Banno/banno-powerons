PowerOn Name:       BANNO.XFRTODQLOAN.RPT
Copyright 2021 Jack Henry and Associates


When a transfer from a share to a loan is created in Banno, the transfer record appears under the
share as a share to loan transfer. In daily posting, share to loan transfers do not occur if the 
loan is delinquent. This report PowerOn provides a listing of share to loan transfers where the 
loan is delinquent. This allows collections to review the accounts from the report. 


This PowerOn is run from batch control, PowerOn generator. PowerOn prompts user for the start
and end dates, systemdate - 1 day is default if nothing is entered for each. Date range entered
selects the transfers that would have occured within the range. The report "Share Transfers to 
DQ Loans" is created in Print Control for review. 


Report Output: From Account; From ID; From Share Owner Name; To Account; To ID; Loan
               Account Owner Name; Due Date; Transfer Date


For more information check
https://github.com/Banno/banno-powerons

Banno is not responsible for any modifications to this file
made by unauthorized personnel.