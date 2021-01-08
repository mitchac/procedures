
### Goal 
The goal of this procedure is to set up an AWS batch environment in which wdl workflows can be launched with Cromwell. 

### Steps

### Set up new AWS account

https://github.com/mitchac/procedures/blob/0a0d5c6162354f74d44f53b5de74604fdc1c1754/aws_new_account.md

### Install the AWS CLI tool to your linux / OSX terminal

https://github.com/mitchac/procedures/blob/9fa1867278556eaed1edf8a5eebc6a2fd71e9ec0/aws_cli_setup.md

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
aws cloudformation describe-stack-resources --stack-name vpcstack2 | grep vpc-
aws cloudformation describe-stack-resources --stack-name vpcstack2 | grep subnet 
```

### Configure AWS batch environment

https://github.com/mitchac/procedures/blob/83cc8107f0fb44726ca00c248a2f61a10f25640f/aws_batch_setup.md


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
```
touch hello.wdl
```
copy file contents from..
https://github.com/mitchac/wdlhelloworld/blob/b0a13f9ac8266ea3528fdca548a8923f18c533b1/hello.wdl
```
touch hello.json
```
copy file contents below..

{
    "hello.SRA_accession_num": "SRR12118866"
}

run the workflow with the following command..

```
curl -X POST "http://localhost:8000/api/workflows/v1" \
-H  "accept: application/json" \
-F "workflowSource=@hello.wdl" \
-F "workflowInputs=@hello.json"
```
You can monitor the progress of your job by going to the AWS batch dashboard at the following link.

https://console.aws.amazon.com/batch/v2/home?region=us-east-1#dashboard

after a few minute you should see execution outputs in 
https://s3.console.aws.amazon.com/s3/buckets/emriuom?region=us-east-1&prefix=cromwell-execution/hello/&showversions=false

nb nothing is logged to cloudwatch in this particular workflow.

### notes
make sure do all in single zone 
