
### Goal 
The goal of this procedure is to set up an AWS batch environment in which wdl workflows can be launched with Cromwell. 

### Steps

### Set up new AWS account

https://github.com/mitchac/procedures/blob/e1f097a497921aac422c929d28fdbaf931aaa210/aws_new_account.md

### Install the AWS CLI tool to your linux / OSX terminal

https://github.com/mitchac/procedures/blob/9fa1867278556eaed1edf8a5eebc6a2fd71e9ec0/aws_cli_setup.md

### Additional doco optional

NB From this point on, this procedure will follow closely (with a few key differences) to the information provided at the following link. 

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

### Create S3 bucket

Run the following command in your terminal
```
aws s3api create-bucket --bucket my-bucket --region us-east-1
```


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

### Configure Nextflow-specific changes to AWS batch environment

Run the following command in your terminal
```
aws cloudformation create-stack \
--stack-name nextflow-resources \
--template-url  https://aws-genomics-workflows.s3.amazonaws.com/latest/templates/nextflow/nextflow-resources.template.yaml \
--parameters \
ParameterKey=GWFCoreNamespace,ParameterValue=gwfcore \
ParameterKey=TemplateRootUrl,ParameterValue=https://aws-genomics-workflows.s3.amazonaws.com/v3.0.2/templates \
ParameterKey=Namespace,ParameterValue=nfres \
--capabilities CAPABILITY_IAM
```

### Run a test workflow to test your AWS batch environment

Run the following command in your terminal
```
aws batch submit-job \
--job-name SRR12118866 \
--job-queue default-gwfcore \
--job-definition nextflow-nfres \
--container-overrides command=mitchac/nextflow-ascp
```

You can monitor the progress of your job by going to the AWS batch dashboard at the following link.

https://console.aws.amazon.com/batch/v2/home?region=us-east-1#dashboard

After a minute or two, real-time logging for your workflow should be available at the following link. 

https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#logsV2:log-groups/log-group/$252Faws$252Fbatch$252Fjob



### notes
make sure do all in single zone 
