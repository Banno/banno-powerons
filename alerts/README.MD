##Alerts PowerOn Service

The Banno Alert service uses PowerOns to query Episys for transactions that may trigger an alert.  Once a user sets up a share or loan alert, Banno adds the member number plus share or loan ID to the MEMBERDATA.DAT data file. The PowerOns use that list to gather transactions to hand back to Banno. This process runs every 10-15 minutes.  

###PowerOn Name(s)
•	BANNO.ALERT.TRANSACTIONS.V1
•	BANNO.ALERT.TRANSFILE.V1

The BANNO.ALERT.TRANSACTIONS.V1 runs every time Episys is queried.  If the returning transaction count is more than 9500 transactions, then Banno uses BANNO.ALERT.TRANSFILE.V1 repeatedly until all transactions are returned.

###Troubleshooting
1.	Verify PowerOn installation
    a.	PowerOns are in your PowerOn library named exactly as shown above.
    b.	The PowerOns are installed for demand use.
    c.	Check SymXchange Parameters 
         i.	Go to Episys Parameter Manager
        ii.	Go to SymXchange Parameters
       iii.	Select Banno
       iv.	Select Common 
        v.	The PowerOns should be listed there and they are set to Individual
        
2.	Check the return lines
    a.	Go to Episys Parameter Manager
    b.	Go to SymXchange Parameters
    c.	Select Banno
    d.	Select appropriate SymXchange Client Number
    e.	Select Client Parameter
    f.	Ensure the following items are set to 10,000
         i.	Maximum Response Lines
        ii.	Maximum Specfile Lines
       iii.	Maximum All Field Count
       
3.	Refresh Banno
    a.	Go to Device Control
    b.	From the drop down, select SymXchange
    c.	Select Banno
    d.	Click Refresh
    
4.	If testing, ensure the system date on your test SYM is current

5.	Check for the user’s account number is listed in the MEMBERDATA.DAT data file

