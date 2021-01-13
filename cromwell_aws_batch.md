
### Goal 
The goal of this procedure is to set up an AWS batch environment in which wdl workflows can be launched with Cromwell. 

### Steps

### Set up new AWS account

https://github.com/mitchac/procedures/blob/6481d404f7466ac5a65282b44c939a7dcb2855dc/aws_new_account.md

### Install the AWS CLI tool to your linux / OSX terminal

https://github.com/mitchac/procedures/blob/3e29da3ae234a7f755a4bd9ea313e970e03d9b51/aws_cli_setup.md

### Additional doco optional

The next few steps will follow closely (with a few key differences) the information provided at the following link. 

https://docs.opendata.aws/genomics-workflows/orchestration/cromwell/cromwell-overview/

For additional clarification of the purpose of the following steps it is recommended to consult this link. 

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
aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='VPCID'].OutputValue" --output text
aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet1ID'].OutputValue" --output text
aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet2ID'].OutputValue" --output text
```

### Configure AWS batch environment

https://github.com/mitchac/procedures/blob/da3d7dda3b315a97e80a1fd2fab59fbcff3eb1ee/aws_batch_setup.md


### Configure Cromwell-specific changes to AWS batch environment

https://github.com/mitchac/procedures/blob/5306eaddc057037268c1eaf03c3ba134eb53872e/install_cromwell.md

### Run a test workflow

Go to https://console.aws.amazon.com/cloudformation/home?region=us-east-1

Click on your completed cromwell stack in the list.

Click on resources, scroll through the list and click on 'EC2Instance' to be taken to the EC2 console.

Click on the instance id

Click on the connect tab

Click on the SSH client tab 

Copy the ssh command from this section and run it in your terminal. This command should look like..

```
ssh -i "my_key_name.pem" ec2-user@ec2{my instance ip}.compute-1.amazonaws.com
```
Get test wdl script 
```
sudo yum update -y
sudo yum install git -y
git clone https://github.com/mitchac/wdlhelloworld.git
git reset cbc0d4c --hard
```
run the workflow with the following command..
```
curl -X POST "http://localhost:8000/api/workflows/v1" \
-H  "accept: application/json" \
-F "workflowSource=@wdlhelloworld/hello.wdl" \
-F "workflowInputs=@wdlhelloworld/hello.json" \
-F "workflowOptions=@wdlhelloworld/options.json"
```
You can monitor the progress of your job by going to the AWS batch dashboard at the following link.

https://console.aws.amazon.com/batch/v2/home?region=us-east-1#dashboard

after a few minute you should see execution outputs in 
https://s3.console.aws.amazon.com/s3/buckets/emriuom?region=us-east-1&prefix=cromwell-execution/hello/&showversions=false

nb nothing is logged to cloudwatch in this particular workflow.

### notes
make sure do all in single zone 
