# Link user to Episys Name record
## PowerOn Overview
The PowerOn `BANNO.DATABASE.CHECK.V1.POW` runs each time the user logs into, or enrolls into, Banno. The program attempts to bridge the gap between a Preference record (associated with the user log-in) and their 
name record. The Episys core does not currently have this association. The program creates or updates an account level tracking record type 08 and stores elements from the Preference record (locator), Banno 
database (Banno user ID), and links for both name information (i.e. name, birthdate, etc.) and address information. If the credit union wants Banno initiated address changes to update directly to the core name 
record, the link process is essential. In the future, there may be other features also relying on the account tracking type 08.


## PowerOn Detail
**Enrollment** *(new users)*

During enrollment, Banno pushes the Banno user ID, Preference locator number, the member link number, and the address link number to the PowerOn (POW). In the PowerOn programming, this is called the 
CREATETRACKING process. The PowerOn programming will create a new account level tracking 08 and add the provided information.

**Login** *(existing in Episys)*

With each successful login, Banno pushes the Banno user ID, Preference locator number, and a list of name types allowed (per the enrollment settings) to the PowerOn. In the PowerOn programming, this is called 
the LINKTRACKING process. The programming process verifies, creates, and updates an Account Tracking 08 record (NetTeller) as needed. Each Preference record should have a unique Tracking record. At this time, 
the tracking fields used are hidden. In Episys, the user cannot see the Banno used tracking fields when looking at the tracking record. To do so requires an Episys change. However, the credit union can use a 
PowerOn to see the fields and update the data.

The information stored in the tracking record type 08 is stored in the following fields:
-   USERCHAR12: Preference Locator
-   USERCHAR13: Banno User ID
-   USERCHAR14: Member Number Link
-   USERCHAR15: Member Address Number Link

Once the program creates or updates a tracking record type 08 and stores the preference locator and the Banno user ID, the PowerOn then attempts to find the Name record related to that user. If there is only one 
applicable name record found, the link is complete and the tracking record is updated. If there is more than one, no linking is done, but there will be a note record created with the attempt time and date. This 
note record can then be used by the CU staff for reference

When the program is searching for a name match, Banno provides the enrollment name types to the PowerOn. Then the PowerOn searches the account-level Name records with those types. When selecting the name record 
to link, there is a hierarchy of name types:
 1. Prime (name type 0)
 2. Joint (name type 1)
 3. Custodian (name type 5)
 4. Trustee (name type 6)
 5. Responsible individual (name type 7)
 6. Power of Attorney (name type 8)
 7. Authorized signer (name type 9)

The name types that the CU is allowing the program to match against are defined in the "allowable_name_type_for_enrollment" setting in Banno People

## PowerOn installation

An Overview
* Install the program to the REPWRITERSPECS directory*
* Add the program to SymXchange Common parameters and refresh SymXchange

In Detail
* [Click here](https://github.com/Banno/banno-powerons) for step-by-step instructions on installing the PowerOn on your host.
* You may also open a case, requesting installation of the program for you.
## Required files
* PowerOn Name:  `BANNO.DATABASE.CHECK.V1.POW`
## Supported SymXchange web consoles
* PowerOnService - 2017.01 V1
* PowerOnService - 2018.00
* PowerOnService - 2018.01
