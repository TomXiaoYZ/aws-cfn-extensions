# aws-cfn-extensions

## Why we have this project

The original quickstart extension repos have been deleted and the go1.x runtime was retired this year.
This project is fork from the original aws quickstart repos and upgraded to the latest provided.al2 runtime.

## How to use
### Example: AWSQS::EKS::Cluster

```
cd quickstart-amazon-eks-cluster-resource-provider
make package
```


After compiling, upload the zip file (awsqs-eks-cluster.zip) to any of your s3 bucket.
In your cloudformation template, use the following code to install the extension as private extension.


```
AWSQSEKSClusterResourceVersion:
  Type: AWS::CloudFormation::ResourceVersion
    Properties:
      TypeName: AWSQS::EKS::Cluster
      ExecutionRoleArn: !GetAtt AWSQSEKSClusterExecutionRole.Arn
      LoggingConfig:
      LogGroupName: !Join
        - '-'
        - - '/aws/cloudformation/registry/AWSQSEKSClusterLogGroup'
          - !Select [2, !Split [ '/', !Ref AWS::StackId ]]
      LogRoleArn: !GetAtt AWSQSEKSClusterLogDeliveryRole.Arn
      SchemaHandlerPackage: '/path/to/awsqs-eks-cluster.zip'
 
AWSQSEKSClusterResourceDefaultVersion:
  Type: AWS::CloudFormation::ResourceDefaultVersion
  Properties:
    TypeVersionArn: !Ref AWSQSEKSClusterResourceVersion
```