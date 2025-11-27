# AWS CloudFormation Network Template

To use the templates, clone this repo. Create s3 bucket to store the nested stacks:
- vpc.yaml
- subnet.yaml
- internetGateway.yaml
- publicRouteTable.yaml
- privateRouteTable.yaml

Update AssembleNetwork.yaml with preferred cidr block for vpc, subnets, instanceTenancy, availability zones where subnets will be created, and S3 bucket keys for the nested templates. 
Defaults will apply as stated in the top level template if not overridden.

You can validate the templates using the command below:
```
aws cloudformation validate-template \
	--template-body file://filename.yaml \
	--region <your_region>
```

Run this command to package the templates and generate package template that will be used to deploy the stack 
```
aws cloudformation package \ 
	--template-file AssembleNetwork.yaml \ 
	--s3-bucket <bucket_name> \  
	--output-template-file packaged-template.yaml \  
	--region <your_region> \  
	--profile <profile with right permissions>  \
	--force-upload \
	--debug
```

Deploy the generated template to create the stack with the command below: 
```
aws cloudformation deploy \
  --template-file packaged-template.yaml \
  --stack-name NetworkStack \
  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
  --profile <profile with right permissions> \
  --region <your_region>
```

In the event that stack create/update fails, you can run this command to see the list of events and the cause of the failure
```
aws cloudformation describe-stack-events \
 --stack-name <stack_name> \
 --profile <profile with right permissions>
```

When you update a stack and want to view the changes before applying same to update the stack, you need to run the create-change-set command 
```
aws cloudformation create-change-set \
  --stack-name <stack_name> \
  --change-set-name <The name for the change stack> \
  --template-body file://<file name generated from CloudFormation package command> \
  --capabilities CAPABILITY_NAMED_IAM \
  --profile <profile with right permissions>
  --include-nested-stacks
```

Execute this command to view the changes in the change stack 
```
aws cloudformation describe-change-set \
 --change-set-name <The name for the change stack> \
 --stack-name <stack_name> \
 --profile <profile with right permissions>
```

If all changes to be made are acceptable, run the execute-change-set command to create or update the stack 
```
aws cloudformation execute-change-set \
> --stack-name <stack_name> \
> --change-set-name <The name for the change stack> \
> --profile <profile with right permissions>
```