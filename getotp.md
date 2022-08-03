# Multifactor Authentication (MFA) and CRT

Most Salesforce instances use MFA for login. When creating automated test cases, that needs to be handled somehow. The [QForce](https://help.pace.qentinel.com/qwords-reference/current/qwords/_attachments/QForce.html#library-documentation-top) has a keyword that can be used to achieve that. 

## Setup MFA for CRT in Salesforce

The setup will require steps to be completed strictly as mentioned here. When successful, the setup needs to be done only once for each user.

1. Within Salesforce UI, you need to register the One-Time Password Authenticator. This can be done from the Setup section under Users --> Users --> Your user account --> User Detail. See image for the correct setting.

![image](https://user-images.githubusercontent.com/103214685/182679970-f6a43463-9857-474d-92b8-26b43c1b6ee4.png)

2. Click Connect

3. A new page requiring verification will open

<img width="495" alt="image" src="https://user-images.githubusercontent.com/103214685/182681423-e4de18ef-b63f-44c2-88ff-d7879aa8431f.png">

4. You will receive the verification code (6 digits) via email. Fetch the code and type it into the field and click Verify.

5. On the next page you will see a QR code. Right click on the code and select Open Image in New Tab.

<img width="546" alt="image" src="https://user-images.githubusercontent.com/103214685/182682761-6638ad76-4898-40dd-b011-3096b77b1ab7.png">

6.  From the current tab, you will need to scan the QR code using your authentication app. And type in another code from the app and click Connect.

8.  From the newly opened tab with the QR code image you will need to copy the ending part of the URL that contains your secret.

![image](https://user-images.githubusercontent.com/103214685/182683398-8885d724-da43-4ef5-832b-904243343a0e.png)

9. Copy and paste this secret and save it to your CRT variables.

<img width="533" alt="image" src="https://user-images.githubusercontent.com/103214685/182684167-78e41985-c89f-4c58-9a21-810e47686615.png">

## Example script for CRT

Saving the secret to your suite or robot variables is usually a good idea. As for any other secrets and passwords, it is not recommended to use them as plain text. In this example we have now saved the secret as a robot level variable *SF_SECRET*.

As the simplest solution with only a test case, the login can be achieved with the following code. You will most likely want to move this to *** Keywords *** section or a keyword resource file in order to be able to reuse it.

```
*** Test Cases ***

Login to Instance and Check MFA   
    GoTo                  https://yoursalesforceurl.com
    TypeText              Username                ${USERNAME}       
    TypeText              Password                ${PASSWORD}
    ClickText             Log In
    ${mfa_code}=          GetOTP                  ${USERNAME}    ${SF_SECRET}   https://yoursalesforceurl.com
    TypeSecret            Verification Code       ${mfa_code} 
    ClickText             Verify
```
