
# Skip payment PowerOn

This service PowerOn allows the user to select one or more eligible loans and, for a fee, have the loan due date advanced by a month.

The loan eligibility can be dictated by loan type, service code, minimum & maximum payment amount, account and loan level warning codes, loan season (minimum time opened), amount of time since last skip payment, and/or by the number of skip payments already performed within the last 12 months. The fee charged can be determined by setting a base fee and overwriting by account relationship code if applicable. The user will also be able to choose the share which the fees will be assessed against. The share can be limited by share type.

A Letterfile (see the LETTERSPECS folder) contains parameters which the CU can use to establish which loans & share are available, fees, etc.  The letterfile also allows the CU the ability to include custom verbiage to be used as part of the 'Terms and Conditions' agreement which the user must accept in order to process the transaction(s).

[JSON contract](https://github.com/Banno/banno-powerons/tree/master/loan-payment-skip/loan-payment-skip-json-contract.md)
