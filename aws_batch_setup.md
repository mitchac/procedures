### Create S3 bucket for workflow outputs

Run the following command in your terminal
```
aws s3api create-bucket --bucket emriuom- --region us-east-1
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
bash _scripts/deploy.sh --deploy-region us-east-1 --asset-bucket s3://emriuom-cloudformation gwf
```

Replace the VPC and public subnet ids in the following code with the values you captured previously.
Replace the s3 bucket name with your bucket name.
Then run the command in your terminal. 

```
aws cloudformation create-stack \
--stack-name gwfcore \
--template-url  https://emriuom-cloudformation.s3.amazonaws.com/test/templates/gwfcore/gwfcore-root.template.yaml \
--parameters \
ParameterKey=VpcId,ParameterValue={Your VpcId} \
ParameterKey=SubnetIds,ParameterValue={Your public subnet 1}\\,{Your public subnet 2} \
ParameterKey=S3BucketName,ParameterValue={Your s3 bucket} \
ParameterKey=ExistingBucket,ParameterValue=true \
ParameterKey=ArtifactBucketName,ParameterValue=emriuom-cloudformation \
ParameterKey=ArtifactBucketPrefix,ParameterValue=test/artifacts \
ParameterKey=TemplateRootUrl,ParameterValue=https://emriuom-cloudformation.s3.amazonaws.com/test/templates \
--capabilities CAPABILITY_IAM
```
