# Getting Started

## Prerequisits

* [Install Go](https://go.dev/doc/install)

* [Installing SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)

* [Create an AWS Account and configure credentials for terminal](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)

* VpcConfig needs to be configured on [template.yml](template.yml#L35) (Optional, you can also delete this part.)

## Building your function

### For developers on Linux and macOS:
``` shell
# Build handler executable for Linux
GOOS=linux GOARCH=amd64 go build
```

## Local Test w/ SAM CLI
Run the serverless application locally for quick development and testing.

This command uses `template.yml` file as a reference.

``` shell
sam local start-api
```

## Create S3 Bucket & Set the variables

``` shell
export REGION="<REGION>"
export S3_BUCKET="<BUCKET_NAME>"
export STACK_NAME="<STACK_NAME>"

# If you already have a S3 bucket skip below command
aws s3 mb --region $REGION s3://$S3_BUCKET
```

## Package the SAM application

``` shell
sam package \
    --template-file template.yml \
    --s3-bucket $S3_BUCKET \
    --region $REGION \
    --output-template-file packaged.yml
```

## Deploy the SAM application

``` shell
sam deploy \
    --template-file packaged.yml \
    --stack-name $STACK_NAME \
    --capabilities CAPABILITY_IAM \
    --region $REGION \
    --parameter-overrides AppId="test"
```
The `--parameter-override` option is not required, also can be specified on [template.yml](template.yml#L10).

## Clean Up

### Delete the SAM application by deleting the AWS CloudFormation stack

``` shell
sam delete \
    --stack-name $STACK_NAME \
    --region $REGION \
    --no-prompts
```

### Remove all objects in S3 Bucket & Delete S3 Bucket

``` shell
aws s3 rb s3://$S3_BUCKET --force
```

# References:

* [AWS SAM template anatomy](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification-template-anatomy.html)
* [Deleting a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html)
* [Using AWS Lambda with Amazon API Gateway](https://docs.aws.amazon.com/lambda/latest/dg/services-apigateway.html)
* [SAM Template Globals Section](https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst)