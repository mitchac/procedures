### Install Cromwell to AWS batch environment

### Create ssh key 

Save the output of the following command to enable you to log in to your Cromwell server. 
```
aws ec2 create-key-pair --key-name MyKeyPair
```

### Configure Cromwell-specific changes to AWS batch environment

Configure the variables for executing the script
```
export AWS_VPC_ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='VPCID'].OutputValue" --output text)
export AWS_VPC_SUBNET1_ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet1ID'].OutputValue" --output text)
export AWS_VPC_SUBNET2_ID=$(aws cloudformation describe-stacks --stack-name vpcstack --query "Stacks[0].Outputs[?OutputKey=='PublicSubnet2ID'].OutputValue" --output text)
export AWS_CROMWELL_STACKNAME=cromwell
export AWS_CROMWELL_TEMPLATE_URL=https://aws-genomics-workflows.s3.amazonaws.com/latest/templates/cromwell/cromwell-resources.template.yaml 
export AWS_CROMWELL_NAMESPACE=cromwell
export AWS_CROMWELL_GWFCORE_NAMESPACE=gwfcore
export AWS_CROMWELL_KEYNAME=*******
export AWS_CROMWELL_DBPASSWORD=*******
```

Run the following command in your terminal
```
aws cloudformation create-stack \
--stack-name $AWS_CROMWELL_STACKNAME \
--template-url $AWS_CROMWELL_TEMPLATE_URL \
--parameters \
ParameterKey=Namespace,ParameterValue=$AWS_CROMWELL_NAMESPACE \
ParameterKey=GWFCoreNamespace,ParameterValue=$AWS_CROMWELL_GWFCORE_NAMESPACE \
ParameterKey=VpcId,ParameterValue=$AWS_VPC_ID \
ParameterKey=ServerSubnetID,ParameterValue=$AWS_VPC_SUBNET1_ID \
ParameterKey=DBSubnetIDs,ParameterValue=$AWS_VPC_SUBNET1_ID\\,$AWS_VPC_SUBNET2_ID \
ParameterKey=KeyName,ParameterValue=$AWS_CROMWELL_KEYNAME \
ParameterKey=DBPassword,ParameterValue='$AWS_CROMWELL_DBPASSWORD' \
--capabilities CAPABILITY_IAM
```
