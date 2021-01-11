### Install the AWS CLI tool to your linux / OSX terminal

Install the AWS CLI tool either using the instructions provided by AWS at the following link.

https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html

Or using Conda per the following link

https://anaconda.org/conda-forge/awscli

### Create an aws config 

```
touch ~/.aws/credentials
```
add the following lines to this file 
```
[default]
aws_access_key_id={Your access key id}
aws_secret_access_key={Your secret key}
```
```
touch ~/.aws/config
```
add the following lines to this file 
```
[default]
region=us-east-1
```
