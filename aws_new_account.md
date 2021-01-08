### Create AWS administrator account

Go to https://aws.amazon.com/ and click on the 'Create an AWS account' button. 

Complete the process to create an account. 

It is recommended to add 2 factor authentication as part of this process. 

### Add a credit card to your account

Go to https://console.aws.amazon.com/ and login with your new account details.

Go to https://console.aws.amazon.com/billing/ and select 'Payment methods' in the left hand navigation. 

Complete the process to add a credit card. 

### Add a non root user to your account

It is recommended to create a non-root user account for day to day use of AWS in order to avoid credentials for your root account falling into the wrong hands.  

Download the following template file to your computer. This file will automate the creation of your non-root user.

https://github.com/mitchac/cloudformation/blob/85002c45cc47beca9906fba699d40cb4f4b016c4/default-user.template.yaml

Go to https://console.aws.amazon.com/cloudformation

Click on the 'Create Stack' button and select option 'with new resources'

Select option 'Upload a template file'. 

Click on 'Choose file'.

Select the template file you downloaded previously in the file selection popup. 

Click the Next button

Enter a username and password and click Next

Click Next

At the bottom of the page tick any checkboxes in the 'Capabilities' section to enable the creation of appropriate resources and click 'Create stack' button. 

Make a note of the username, password and the security key details.
