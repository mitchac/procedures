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
export AWS-VPC-ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='VPCID'].OutputValue" --output text)
export AWS-VPC-SUBNET1-ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet1ID'].OutputValue" --output text)
export AWS-VPC-SUBNET2-ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet2ID'].OutputValue" --output text)
export AWS-GWFCORE-STACKNAME=gwfcore
export AWS-GWFCORE-TEMPLATE-URL=https://emriuom-cloudformation.s3.amazonaws.com/test/templates/gwfcore/gwfcore-root.template.yaml
export AWS-GWFCORE-S3-BUCKET=emriuom-workflow
export AWS-GWFCORE-ARTIFACT-BUCKET=emriuom-cloudformation
export AWS-GWFCORE-ARTIFACT-BUCKET-PREFIX=test/artifacts
export AWS-GWFCORE-TEMPLATE-ROOT-URL=https://emriuom-cloudformation.s3.amazonaws.com/test/templates
```


Replace the VPC and public subnet ids in the following code with the values you captured previously.
Replace the s3 bucket name with your bucket name.
Then run the command in your terminal. 

```
aws cloudformation create-stack \
--stack-name $AWS-GWFCORE-STACKNAME \
--template-url $AWS-GWFCORE-TEMPLATE-URL  \
--parameters \
ParameterKey=VpcId,ParameterValue=$AWSVPCID \
ParameterKey=SubnetIds,ParameterValue=$AWSSUBNET1ID\\,$AWSSUBNET1ID \
ParameterKey=S3BucketName,ParameterValue=$AWS-GWFCORE-S3-BUCKET \
ParameterKey=ExistingBucket,ParameterValue=true \
ParameterKey=ArtifactBucketName,ParameterValue=$AWS-GWFCORE-S3-BUCKET \
ParameterKey=ArtifactBucketPrefix,ParameterValue=$AWS-GWFCORE-ARTIFACT-BUCKET-PREFIX\
ParameterKey=TemplateRootUrl,ParameterValue=$AWS-GWFCORE-TEMPLATE-ROOT-URL \
--capabilities CAPABILITY_IAM
```
