
### Goal 
The goal of this procedure is to set up an AWS batch environment into which Nextflow jobs can be launched. 

### Steps

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

https://github.com/mitchac/cloudformation/blob/a486321a5f49bf9972bd16f9372b5f274b23c3be/default-user.template.yaml

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

### Install the AWS CLI tool to your linux / OSX terminal

Install the AWS CLI tool either using the instructions provided by AWS at the following link.

https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html

Or using Conda per the following link

https://anaconda.org/conda-forge/awscli

### Export your key and default AWS region to your shell environment

export AWS_ACCESS_KEY_ID={Your access key id}
export AWS_SECRET_ACCESS_KEY={Your secret key}
export AWS_DEFAULT_REGION=us-east-1


