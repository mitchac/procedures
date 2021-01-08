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
--stack-name nextflow-resources \
--template-url  https://aws-genomics-workflows.s3.amazonaws.com/latest/templates/nextflow/nextflow-resources.template.yaml \
--parameters \
ParameterKey=GWFCoreNamespace,ParameterValue=gwfcore \
ParameterKey=TemplateRootUrl,ParameterValue=https://aws-genomics-workflows.s3.amazonaws.com/v3.0.2/templates \
ParameterKey=Namespace,ParameterValue=nfres \
--capabilities CAPABILITY_IAM
```
