
### Goal 
The goal of this procedure is to set up an AWS batch environment in which wdl workflows can be launched with Cromwell. 

### Steps

### Set up new AWS account

https://github.com/mitchac/procedures/blob/5133a65bbc0c837eb9c4b560d87a43da948384dd/aws_new_account.md

### Install the AWS CLI tool to your linux / OSX terminal

https://github.com/mitchac/procedures/blob/3e29da3ae234a7f755a4bd9ea313e970e03d9b51/aws_cli_setup.md

### Additional doco optional

The next few steps will follow closely (with a few key differences) the information provided at the following link. 

https://docs.opendata.aws/genomics-workflows/orchestration/cromwell/cromwell-overview/

For additional clarification of the purpose of the following steps it is recommended to consult this link. 

### Create VPC

https://github.com/mitchac/procedures/blob/e0564e7912ed4473bbc66b54022cdb043d823860/aws_vpc_setup.md

### Configure AWS batch environment

https://github.com/mitchac/procedures/blob/85ec94a93f5492f6a87eacac067a6776e4ddf039/aws_batch_setup.md

### Install Cromwell

https://github.com/mitchac/procedures/blob/af1b65a0647fdb91655b9a9b10c1a86510c4fa69/install_cromwell.md

### Run a test workflow

Go to https://console.aws.amazon.com/cloudformation/home?region=us-east-1

Click on your completed cromwell stack in the list.

Click on resources, scroll through the list and click on 'EC2Instance' to be taken to the EC2 console.

Click on the instance id

Click on the connect tab

Click on the Session manager tab 

Click on connect

This will lanch a new browser tab with a console on your Cromwell head node 

Install git
```
sudo yum update -y
sudo yum install git -y
```
Get test wdl script. 
The following is adapted from https://docs.opendata.aws/genomics-workflows/orchestration/cromwell/cromwell-examples/. 
```
git clone https://github.com/mitchac/wdlhelloworld.git
cd wdlhelloworld
git reset eb6cf52 --hard
cd ..
```
Run test workflow script
```
curl -X POST "http://localhost:8000/api/workflows/v1" \
    -H  "accept: application/json" \
    -F "workflowSource=@wdlhelloworld/simple-hello.wdl"
```
The workflow may take 5-10 minutes to complete. You can monitor the progress of your job by going to the AWS batch dashboard at the following link. 

https://console.aws.amazon.com/batch/v2/home?region=us-east-1#dashboard

You can find the relevant cromwell server logs at the following location. 

https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#logsV2:log-groups/log-group/cromwell-server

### notes
make sure do all in single zone 
