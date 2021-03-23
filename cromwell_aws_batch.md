
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
cd wdlhelloworld
git reset cbc0d4c --hard
cd ..
```
Copy workflow input file to s3

NB complete this step on your main development machine not the Cromwell AWS instance
```
git clone https://github.com/mitchac/wdlhelloworld.git
cd wdlhelloworld
aws s3 cp runlist s3://emriuom-workflow/cromwell_inputs/runlist
```
Run the workflow with the following command..

NB if necessary, modify the path to your input file on s3 in the file hello.json
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
