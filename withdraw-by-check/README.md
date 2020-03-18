# Withdraw by Check PowerOn

This service PowerOn allows the user to withdraw funds, in the form of a check, from an eligible share or loan and have the check mailed to them.
The share or loan eligibility can be dictated by share or loan type and available balance for shares or the cash advance limit for loans. A member can be excluded by account warning code(s) or for specific shares or loans by share or loan warning code(s).

The program will use the system calculated member address (ACCOUNT:PAYEELINE:[1-6]) as the address to which the check will be mailed.

A Letterfile (see the LETTERSPECS folder) contains parameters which the CU can use to establish which loans & shares are available, etc. The letterfile also allows the CU the ability to include custom verbiage to be used as part of the 'Terms and Conditions' agreement which the user must accept in order to process the transaction(s).

[JSON contract](https://github.com/Banno/banno-powerons/blob/master/withdraw-by-check/withdraw-by-check-json-contract.md)
