### Configure AWS batch environment

nb in the following commands I have used S3 bucket names emriuom-workflow and emriuom-cloudformation. You should create buckets with different names and amend the commands accordingly. 

### Create S3 bucket for workflow outputs

Run the following command in your terminal
```
aws s3api create-bucket --bucket emriuom-workflow --region us-east-1
```

### Create public S3 bucket from which the cloudformation template files can be accessed

Run the following command in your terminal
```
aws s3api create-bucket --bucket emriuom-cloudformation --region us-east-1 --acl public-read
```

### Configure AWS batch environment

```
git clone https://github.com/mitchac/aws-genomics-workflows.git
cd aws-genomics-workflows
bash _scripts/deploy.sh --deploy-region us-east-1 --asset-profile default --asset-bucket s3://emriuom-cloudformation test
```
Configure the variables for executing the script
```
export AWS_VPC_ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='VPCID'].OutputValue" --output text)
export AWS_VPC_SUBNET1_ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet1ID'].OutputValue" --output text)
export AWS_VPC_SUBNET2_ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet2ID'].OutputValue" --output text)
export AWS_GWFCORE_STACKNAME=gwfcore
export AWS_GWFCORE_TEMPLATE_URL=https://emriuom-cloudformation.s3.amazonaws.com/test/templates/gwfcore/gwfcore-root.template.yaml
export AWS_GWFCORE_S3_BUCKET=emriuom-workflow
export AWS_GWFCORE_ARTIFACT_BUCKET=emriuom-cloudformation
export AWS_GWFCORE_ARTIFACT_BUCKET_PREFIX=test/artifacts
export AWS_GWFCORE_TEMPLATE_ROOT_URL=https://emriuom-cloudformation.s3.amazonaws.com/test/templates
```

Run the following command in your terminal. 

```
aws cloudformation create-stack \
--stack-name $AWS_GWFCORE_STACKNAME \
--template-url $AWS_GWFCORE_TEMPLATE_URL  \
--parameters \
ParameterKey=VpcId,ParameterValue=$AWS_VPC_ID \
ParameterKey=SubnetIds,ParameterValue=$AWS_VPC_SUBNET1_ID\\,$AWS_VPC_SUBNET1_ID \
ParameterKey=S3BucketName,ParameterValue=$AWS-GWFCORE-S3-BUCKET \
ParameterKey=ExistingBucket,ParameterValue=true \
ParameterKey=ArtifactBucketName,ParameterValue=$AWS_GWFCORE_ARTIFACT_BUCKET \
ParameterKey=ArtifactBucketPrefix,ParameterValue=$AWS_GWFCORE_ARTIFACT_BUCKET_PREFIX \
ParameterKey=TemplateRootUrl,ParameterValue=$AWS_GWFCORE_TEMPLATE_ROOT_URL \
--capabilities CAPABILITY_IAM
```
