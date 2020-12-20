
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

```
export AWS_ACCESS_KEY_ID={Your access key id} \
export AWS_SECRET_ACCESS_KEY={Your secret key} \
export AWS_DEFAULT_REGION=us-east-1
```

### Additional doco optional

NB from this point, this procedure will track closely to the information provided in the following link. 

https://docs.opendata.aws/genomics-workflows/orchestration/nextflow/nextflow-overview/

For additional clarification of the purpose of these steps it is recommended to consult this link. 

### Create VPC

Run the following command in your terminal

```
aws cloudformation create-stack \
--stack-name vpcstack \
--template-url  https://aws-quickstart.s3.amazonaws.com/quickstart-aws-vpc/templates/aws-vpc.template \
--parameters \
ParameterKey=AvailabilityZones,ParameterValue=us-east-1a\\,us-east-1b \
ParameterKey=CreatePrivateSubnets,ParameterValue=false
```

Run the following commands to retrieve the identifiers for your VPC and the public subnets

```
aws cloudformation describe-stack-resources --stack-name vpcstack2 | grep vpc-
aws cloudformation describe-stack-resources --stack-name vpcstack2 | grep subnet 
```

### Create S3 bucket




### Configure AWS batch environment

Replace the VPC and public subnet ids in the following code with the values you captured previously.
Replace the s3 bucket name with your bucket name.
Then run the command in your terminal. 

```
aws cloudformation create-stack \
--stack-name gwfcore \
--template-url  https://emriuom-cf.s3.amazonaws.com/test/templates/gwfcore/gwfcore-root.template.yaml \
--parameters \
ParameterKey=VpcId,ParameterValue={Your VpcId} \
ParameterKey=SubnetIds,ParameterValue={Your public subnet 1}\\,{Your public subnet 2} \
ParameterKey=S3BucketName,ParameterValue={Your s3 bucket} \
ParameterKey=ExistingBucket,ParameterValue=true \
ParameterKey=ArtifactBucketName,ParameterValue=emriuom-cf \
ParameterKey=ArtifactBucketPrefix,ParameterValue=test/artifacts \
ParameterKey=TemplateRootUrl,ParameterValue=https://emriuom-cf.s3.amazonaws.com/test/templates \
--capabilities CAPABILITY_IAM
```

### Configure Nextflow customisations to AWS batch environment


### notes
make sure do all in single zone 
