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
