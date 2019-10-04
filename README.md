# Simple AWS Terraform

Create a super simple AWS instance with VPC.

Steps (these are for Mac, they’ll be similar for Windows – or maybe use Linux-on-Windows (WSL)):
* Download Terraform 0.12.x (and add to your PATH)
* Download AWS CLI
    * https://aws.amazon.com/cli/
* Get your AWS access / secret keys
    * Go to AWS Web UI > IAM > Users > Select your user > Security Credentials (tab) > Create access key (button)
* Create a file `$HOME/.aws/credentials`
```
    [default]
    aws_access_key_id = AKBLAHBLAHBLAHB6U
    aws_secret_access_key = BdYlskjdflksdfljflkjflksjff
```
* Create a file `$HOME/.aws/config` (note, if you use a different region than _eu-west-1_, you’ll need to edit the AMI ID in the simple-aws-terraform.tf)
```
    [default]
    region = eu-west-1
```
* Create a key pair (note, it’ll be created in the region specified in the file above ^^)
```
    aws ec2 create-key-pair --key-name "simple-aws-terraform" --query "KeyMaterial" --output text > simple-aws-terraform.pem
```
* Change the permissions on that private key
```
    chmod 400 test-terraform.pem
```
* Edit the included `variables.auto.tfvars` to have your accessKey + secretKey
* Run
```
    terraform plan
```
* Then
```
    terraform apply
```
* This should, when complete, output the Public_DNS for that nginx instance.
* When it is complete you should be able to browse to the PUBLIC_DNS, or, connect using ssh:
```
    ssh -i simple-aws-terraform.pem ec2-user@<PUBLIC_DNS>
```