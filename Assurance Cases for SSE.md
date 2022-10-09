# Team 1 Assurance Cases for Software Security Engineering: Liberapay

Written by Team 1:
Joel Allou, Mohammad H. Al Huneidi, Miti Mareddy, Justin Robbins, and Nicholas Sabata

Link to Liberapay Github: [Liberapay Project](https://github.com/liberapay/liberapay.com)


## 1. Claims

### 1.1. Claim 1 - Liberapay minimizes unauthorized access to sensitive data
![Claim 1 Diagram](/Images/Assurance_Case_Diagrams/Joel_Assurance_Case.png)

### 1.2. Claim 2 - Liberapay minimizes unauthorized account access
![Claim 2 Diagram](/Images/Assurance_Case_Diagrams/Justin_Robbins-Assurance_CaseV2.jpeg)

### 1.3. Claim 3 - Liberapay minimizes scripted account registration attacks
![Claim 3 Diagram](/Images/Assurance_Case_Diagrams/Mo-AC-3-1.png)

### 1.4. Claim 4 - Liberapay reduces the ability for attackers to impede organization activities
![Claim 4 Diagram](/Images/Assurance_Case_Diagrams/Miti_AC_4_1.png)

### 1.5. Claim 5 - Liberapay minimizes the ability for attackers to interfere with donations
![Claim 5 Diagram](/Images/Assurance_Case_Diagrams/Nicholas_Assurance_Case.png)


## 2. Evidence Alignment

### 2.1. Claim 1 Evidence Alignment

#### *2.1.1. Available Evidence*
*E1* \
Liberapay has an admin interface that allows admins to perform several actions, such as audit log/monitoring activities, viewing email addresses, notifications, payments, rate limits, as well as users, etc.

Along with that, Liberapay also logs actions performed by admins. In this way, if an admin account was ever compromised, Liberapay would be able to analyze the logs to troubleshoot the issue.

*E3* \
Liberapay implements symmetric encryption and decryption to ensure that sensitive data are protected. They currently rely on Fernet, which uses the AES cipher in CBC mode with PKCS7 padding and a 128 bits key. For authentication, they use HMAC-SHA256 with a 128 bits key. ([Crytography Code](https://github.com/liberapay/liberapay.com/blob/fb1dbeac869d235abf25f086cbc5b274931578d1/liberapay/security/crypto.py))

Along with that, [Liberapay](https://liberapay.com/about/privacy) also affirms on their privacy page that all of their connections are encrypted.

*E7* \
Liberapay does not have a vulnerability management process in place. However, they do (on re-occuring occasions) update and patch packages, tools, and softwares.
Below are some examples of times they have updated components:
- [Various fixes based on findings](https://github.com/liberapay/liberapay.com/pull/1766)
- [Patching SEPA Direct Debits to avoid failed Payment](https://github.com/liberapay/liberapay.com/pull/1369)
- [Upgrading urllib3](https://github.com/liberapay/liberapay.com/pull/1487)
- [Impersonation vulnerability](https://github.com/liberapay/liberapay.com/pull/1364)

Liberapay also had a discussion around implementing automated patches, but it has yet to be done. ([Automated Patches Discussion](https://github.com/liberapay/liberapay.com/issues/1305))

Lastly, Liberapay also has a hacker bounty program where they invite anyone to flag a potential vulnerability. ([Hacker Bounty Website](https://github.com/liberapay/liberapay.com/issues/549))

*E10* \
Liberapay has an admin page and tools that allow admins to perform various actions such as blacklisting suspicious accounts, altering the database, and more. To limit damage in the case where an admin account is compromised, Liberapay has instilled rate limit on the admin accounts as well. ([Rate limit For Admin Actions](https://github.com/liberapay/liberapay.com/pull/1379))

#### *2.1.2. Unavailable/Insufficient Evidence*
*E2* \
Liberapay has done some work to allow database admins to set/revoke privileges, but they still have some ways to go. As this [open request](https://github.com/liberapay/liberapay.com/issues/1312) suggests, Liberapay has built mechanisms to protect against the deletion of an entire production database, but it is still possible to drop or truncate entire tables. Liberapay still needs to define proper user roles. 

*E4* \
Liberapay has an open request to leverage Cloudflare's API to implement a "firewall" by telling Cloudflare to challenge traffic from specific IP addresses. However, that request was opened in 2017 and has yet to be completed. ([Firewall Request](https://github.com/liberapay/liberapay.com/issues/709))

*E5* \
After careful investigation into Liberapay's code base, it does not look like Liberapay uses dynamic SQL. Because the lack of dynamic SQL does not substitute for the guarantee that Liberapay does not have any, we cannot completely support this evidence.

However, Liberapay has taken some measures to reduce SQL injection, as showcased in this [request](https://github.com/liberapay/liberapay.com/issues/559).

*E6* \
Although Liberapay does not use Django validation for input validation, they do validate certain aspects of input. However, there is insufficient evidence to claim that all inputs are validated.
- [Example of implemented input validation](https://github.com/liberapay/liberapay.com/issues/1480)
- [Example of yet to be implemented input validation](https://github.com/liberapay/liberapay.com/issues/70)

*E8* \
Unfortunately, Liberapay's code base and website do not provide any indication that they have packet level authentication to prevent packet spoofing. Consequently, there is not sufficient data for this evidence.

*E9* \
To succesfully create a user's account, Liberapay does employ strong password policies. However, when it comes to password policies to access decryption keys, there wasn't substantial information to support that evidence.

However, as suggested by this [request](https://github.com/liberapay/liberapay.com/issues/1673), Liberapay seems to have a secret vault/manager where they store encryption keys, which could potentially also be protected by passwords.


### 2.2. Claim 2 Evidence Alignment

#### *2.2.1. Available Evidence*
*E2* \
Liberapay leverages simplates to generate many different types of emails for different situations. These simplates act as a standardization that can help reduce the chance that poorly constructed phishing emails will be able to spoof Liberapay's emails.
- Email Simplates: [https://github.com/liberapay/liberapay.com/tree/master/emails](https://github.com/liberapay/liberapay.com/tree/master/emails)

*E3* \
Liberapay implements rate limiting, which restricts the number of times that password logins, email logins, account verifications, and account creations can be performed in an hour. This ensures that attackers cannot merely guess the login information of a user via brute force. Full details about the policy can be seen in the below issue documentation.
- Liberapay rate limiting: [https://github.com/liberapay/liberapay.com/pull/699](https://github.com/liberapay/liberapay.com/pull/699)

*E4, E6, & E9* \
Liberapay leverages https, ensuring that their communications are encrypted according to the TLS encryption algorithm. This also means that their session keys are generated from random data at the time of session initialization. These keys are also short-lived, ensuring that even if the attackers acquire a key through theft, they will not be able to use it for long.
- Liberapay website (notice the use of https): [https://liberapay.com/](https://liberapay.com/)

*E5* \
An argument for the security of Liberapay's sensitive data can be found in Sections 1.1. and 2.1. of this document.

#### *2.2.2. Unavailable/Insufficient Evidence*
*E1 & E7* \
Liberapay is in the process of implementing a time-based one-time password two-factor authentication system. When it is complete, it will greatly improve the security of Liberapay's login services. Until then, attackers with user login information are not prevented from logging in using stolen user credentials. This represents a significant threat to Liberapay, as many users can be prone to using the same password across multiple websites, whose security cannot be guaranteed by Liberapay.
- Two-factor authentication issue on Liberapay's GitHub: [https://github.com/liberapay/liberapay.com/issues/926](https://github.com/liberapay/liberapay.com/issues/926)

*E8* \
Liberapay's implementation of logging in and email sending does not actually send links that the user can use to log into the website automatically. These links are primarily used for account recovery and verification. These emails are only sent on request, so users will not receive emails with links constantly. However, they still represent a gap between the diagram and reality, as these links do require the user to insert login information afterward. This represents a possible vector for spoofing a recovery or verification email to acquire user login data. Additionally, recurring donation reminders also contain links to the pages of the organization the user agreed to donate to, providing another possible attack vector. Overall, the emails could be made more secure by minimizing the amount of links, although I doubt the project leaders would like to trade the user-friendliness of the current system for a more secure system with minimized links.
- Donation reminder links: [https://github.com/liberapay/liberapay.com/blob/master/emails/donate_reminder~v2.spt](https://github.com/liberapay/liberapay.com/blob/master/emails/donate_reminder~v2.spt)
- Account recovery / login request email: [https://github.com/liberapay/liberapay.com/blob/master/emails/login_link.spt](https://github.com/liberapay/liberapay.com/blob/master/emails/login_link.spt)
- Account verification email: [https://github.com/liberapay/liberapay.com/blob/master/emails/verification.spt](https://github.com/liberapay/liberapay.com/blob/master/emails/verification.spt)

*E10* \
The Liberapay project has a long history of trying to plug XSS vulnerabilities in their website, and they have many issues on their project page because of it. The data is there, but it also shows that there are a few XSS issues that remain, representing a gap between the diagram and the actual evidence.
- XSS issues page: [https://github.com/liberapay/liberapay.com/issues?q=xss+](https://github.com/liberapay/liberapay.com/issues?q=xss+)

*E11* \
Unfortunately, it seems that no concerted effort has been made to reduce the amount of vulnerable dynamic HTML used in the website. This means that no report has been created on the analysis of Liberapay's use of dynamic HTML. Likely some of the vulnerabilities have been patched as a result of the XSS vulnerability plugging already done on the website, but the use of dynamic HTML is not a focus of Liberapay's development efforts. This lack of documentation conflicts with the assurance case presented in Claim 2.


### 2.3. Claim 3 Evidence Alignment

#### *2.3.1. Available Evidence*
No evidence was found to support the claims discussed in the assurance case diagram for Claim 3.

#### *2.3.2. Unavailable/Insufficient Evidence*
*E1 & E2* \
Liberapay does not leverage CAPTCHA technologies in the registration process. The current process being utilized is a simple one that only requires the user to provide an email and a password (with no limits in place). Therefore, it's easy for an attacker to register multiple accounts with a script that could potentially flood the database with spam accounts. No issues have been created in the Liberapay project to address this matter, thus it'd be beneficial to point out this security threat to Liberapay so that they can implement it. As addressed in the assurance case diagram, CAPTCHA technologies would greatly reduce the ability of attackers to quickly register accounts.

*E3* \
There is no evidence that Liberapay has any registration limits in place. From analysis and investigation, we've determined that it's possible to create as many accounts as desired in a short-span of time since no timeouts were in place. Liberapay would greatly benefit from a registration limit to hinder attackers' ability in flooding the database with dummy accounts.

### 2.4. Claim 4 Evidence Alignment

#### *2.4.1. Available Evidence*
*E1 & E11* \
An argument for the claims "Liberapay has account protections" and "Liberapay reduces unauthorized acccess to data" can be found in Sections 1.1 and 1.2 above.

*E2* \
Evidence exists to suggest that Liberapay currently implements organization level acccount protections within their participant system backend service. This involves a mix of permission restrictions and validations centered around 'elsewhere accounts', 'duplicate accounts', and 'elevation' restrictions.
- Backend system for participant code (including account protections): [Participant.Py](https://github.com/liberapay/liberapay.com/blob/ca814ed2478108b119fef4498a8ee521a5553914/liberapay/models/participant.py)

*E3* \
Liberapay requires business accounts to be validated when registering with the service. This process is evident when trying to elevate an individual account to an organization level account.
- Validation simplate: [Identity Validation Simplate](https://github.com/liberapay/liberapay.com/blob/89cad02be0bdc42eafb02df2e38aff6535339095/www/%25username/identity-docs.spt)
- Business signup page (note: only available with account): [Business Signup Page](https://liberapay.com/mmrdy/receiving)

*E4* \
Liberapay implements logging of donation histories for an organization/account. Their implementation allows for histories of organizations created by elevating individual accounts as well as newly-formed organizations.
- Python implementation of history tracking: [History.Py](https://github.com/liberapay/liberapay.com/blob/40454cf8d899d08f8fe28559fc40a685a4033f94/liberapay/utils/history.py)

*E5* \
Liberapay incorporates viewing an organization's donation history over its entire lifecycle. This can be achieved either through viewing the organization card on the search page, or by viewing an organization's income history within its profile page.
- View of multiple organizations and income per week on their cards: [Organization Page](https://liberapay.com/explore/organizations)
- Select "view income history" at the bottom of the page: [Liberapay Income History View](https://liberapay.com/LiberapayOrg/)

*E6* \
Liberapay leverages quarantining of payments in order to protect against premature disbursement as well as transaction-related fraud activities. This protection feature is evident through test cases as well as a simplate. The simplate is utilized by Liberapay to send standardized emails to users notifying them of quarantined funds. 
- Test case showcasing quarantining feature: [Quarantine Test](https://github.com/liberapay/liberapay.com/blob/0ec40ad7527bbb446f59c61e53d856352b7a6d1e/liberapay/testing/__init__.py)
- Notification simplate for quarantined funds warning: [Quarantine Simplate](https://github.com/liberapay/liberapay.com/blob/a5290b528c5d7e0f2191208305eb3d1b1b8889ad/www/about/money.spt)

*E8 & E9* \
Liberapay leverages team dashboards in order to visually represent the overall history of donations made to a team as well as the aggregate disbursement to team members. This is evident in pull request no.68, which shows the addition of a team dashboard feature into Liberapay. This process is also visible when looking at a team's page and in the backend code — specifically the membership simplate.
- Pull request no.68 for Team Dashboard feature: [Pull No.68](https://github.com/liberapay/liberapay.com/pull/68)
- Team Dashboard (note: only available with account): [Team Dashboard Page](https://liberapay.com/mmrdy/receiving/#teams)
- Example team dashboard for Liberapay: [Liberapay Team Dashboard](https://liberapay.com/Liberapay/income/)
- Membership simplate: [Membership Simplate](https://github.com/liberapay/liberapay.com/blob/3c77591ccf38093732c555ff588b62932857a9cc/www/%25username/membership/%25action.spt)

#### *2.4.2. Unavailable/Insufficient Evidence*
*E7* \
Currently, Liberapay does not support or provide a feature to view account activity apart from rough metrics on donations for teams. There is no supported feature currently to show team member activity or their histories. However, there is a recent issue (no.482) that was created with the aim to address this issue.
- Ledger history issue page: [Issue No.482](https://github.com/liberapay/liberapay.com/issues/482)

*E10* \
The Liberapay project has been continously updating their payment handling services in order to keep up and meet the standards of their two payment providers: Stripe and Paypal. Currently, the payment processing for Stripe and Paypal are undergoing changes which make the business verification occur external to the system of interest. However, there is an open issue which suggests strong interest and action towards integrating such a service into Liberapay.
- Stripe/Paypal linking process (note: only available with account): [Payment Processing Signup Page](https://liberapay.com/mmrdy/payment)
- Stripe account issue no.1186: [Issue No.1186](https://github.com/liberapay/liberapay.com/issues/1186)


### 2.5. Claim 5 Evidence Alignment

#### *2.5.1. Available Evidence*
*E1 & E3* \
Liberapay implements URL authentication on the payment form in order to prevent potential attackers from modifying the URL contents and accessing restricted payment-related information. Regarding query string parameters on the payment form, Liberapay appends 'beneficiary' and 'method' query strings that must be valid for the URL to successfully open. Here, the 'beneficiary' parameter is associated with a targeted creator and the 'method' parameter is associated with the payment method (card or PayPal). When users either attempt to change the beneficiary or make the method invalid in the website's URL, Liberapay will then direct them to a bad request page. In addition to checking the query string values, Liberapay also authenticates for a matching profile name embedded in the payment form's URL so that attackers cannot impersonate other existing users when making online donations.
- Liberapay payment form URL example: https://liberapay.com/profilename/giving/pay/stripe/?beneficiary=3&method=card

*E2 & E5* \
Liberapay cross-matches with the user database when verifying whether or not a specific user exists. Regarding matching user validation, Liberapay checks if the profile name is an existing user in the database and then references the web cache/cookies to determine whether the current user matches the provided profile name or not. Additionally, when processing the payments from user donations, Liberapay associates these payments with specific users from their stored database.

*E4* \
Along with all of the authentication being performed on the payment form's URL as mentioned for E1 & E3 above, Liberapay also verifies that no URL redirects are potentially being implemented by attackers as a means to spoof the payment form and steal information through redirecting donors to a false payment page.

*E6* \
When requiring the user to enter his/her card payment details, Liberpay validates the card information for legitmacy and alerts the user when false card details are being provided.

#### *2.5.2. Unavailable/Insufficient Evidence*
Evidence was found to support each of the claims discussed in the assurance case diagram for Claim 5.


## 3. GitHub Information

Here are links to our Github Pages: \
Master Branch: [Home Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1) \
Issues Page: [Issues Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/issues) \
Project Board: [Project Board](https://github.com/JustinRobbins7/CSCI-8420-Team-1/projects/3)

Contribution Records: \
Github Pulse: [Pulse Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/pulse) \
Github Contributors: [Contributors Page](https://github.com/JustinRobbins7/CSCI-8420-Team-1/graphs/contributors) 


## 4. Teamwork Reflection

For this deliverable, all team members continued to meet regularly and stay in contact with each other. Each team member was tasked with identifying one unique claim that represented core aspects of the Liberapay system. We met prior to product delivery in order to align our diagrams and analyses with each other. We also proceeded to divide each remaining task among team members for the second section of this deliverable. With the groundwork set up, the assignment progressed smoothly with no conflicts or major impediments.

No major issues occurred during the build and finalization of this deliverable, but a few minor issues occurred in regards to overlapping assurance case diagrams. After meeting with Dr. Ghandi, we were able to sort out this issue with references to other team member analyses. Moving forward, we plan to continue our weekly team meetings and keep everyone up-to-date on both Github and other communication platforms.
