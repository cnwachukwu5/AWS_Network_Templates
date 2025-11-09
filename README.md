# AWS CloudFormation Network Template

To use the templates, clone this repo. Create s3 bucket to store the nested stacks:
- vpc.yaml
- subnet.yaml
- internetGateway.yaml
- publicRouteTable.yaml
- privateRouteTable.yaml

Update AssembleNetwork.yaml with preferred cidr block for vpc, subnets, instanceTenancy, availability zones where subnets will be created, and S3 bucket keys for the nested templates. 
Defaults will apply as stated in the top level template if not overridden.
