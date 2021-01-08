### Install Cromwell to AWS batch environment

### Create ssh key 

Save the output of the following command to enable you to log in to your Cromwell server. 
```
aws ec2 create-key-pair --key-name MyKeyPair
```

### Configure Cromwell-specific changes to AWS batch environment

Run the following command in your terminal
```
aws cloudformation create-stack \
--stack-name cromwell \
--template-url  https://aws-genomics-workflows.s3.amazonaws.com/latest/templates/cromwell/cromwell-resources.template.yaml \
--parameters \
ParameterKey=Namespace,ParameterValue=cromwell \
ParameterKey=GWFCoreNamespace,ParameterValue=gwfcore \
ParameterKey=VpcId,ParameterValue={Your VpcId} \
ParameterKey=ServerSubnetID,ParameterValue={Your public subnet 1} \
ParameterKey=DBSubnetIDs,ParameterValue={Your public subnet 1}\\,{Your public subnet 2} \
ParameterKey=KeyName,ParameterValue={Your ssh key name} \
ParameterKey=DBPassword,ParameterValue='{Create a db password}' \
--capabilities CAPABILITY_IAM
```
