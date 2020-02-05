# Loan payoff calculator
The Loan payoff calculator allows the end-user to estimate their loan payoff amount with included interests and fees. By allowing the customer to retrieve this quote from within Banno, we can save you time and money and enable the customer to make smarter spending decisions.

Loan payoffs are only available on in-house loans, the payoff option will not display on any 3rd party loans; both Account tracking and External Loan record types. Credit card loans will not display the interest rate as they typically have different interest rates for different portions of the balance.

The configuration file gives you the ability to configure: 
* Eligible loan types
* How many days into the future can a payoff be asked for
* Account-level warnings that should prevent a payoff request
* Loan level warnings that should prevent a payoff request
* Real Estate secured loans (i.e. mortgages, HELOCS, etc)
* Boat secured loans and the loan tracking set up for collateral
* Vehicle secured loans and the loan tracking set up for collateral
* Deposit or certificate secured loan types
* Payoff terms and conditions statement

## PowerOn installation
[Click here](https://github.com/Banno/banno-powerons) to learn more about how to install the PowerOn on your host. 

## Required files
* PowerOn Name:  `BANNO.LOAN.PAYOFF.V1.POW`
* Letter file Name:   `BANNO.LOAN.PAYOFF.V1.CFG`

## Supported SymXchange web consoles
* PowerOnService - 2017.01 V1
* PowerOnService - 2018.00
* PowerOnService - 2018.01

_In order for your PowerOn to show-up you'll need to contact your support or implementation leader to turn on the Loan Payoff linktype._


