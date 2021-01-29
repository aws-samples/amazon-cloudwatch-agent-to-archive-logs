# Amazon CloudWatch Agent to archive logs Sample

This demonstration has the purpose to present how we can use Amazon CloudWatch Agent to collect logs from an Apache Web Server and save it in a S3 Bucket using Amazon CloudWatch Logs Subscription with Kinesis Firehose Delivery Stream. Also we have created a lifecycle configuration rule for the S3 bucket to transition objects to Glacier in order to optimize some storage costs.

# Prerequisites

- AWS CLI
- AWS account
- Don't have Amazon CloudWatch Log Groups for an existing Apache called "access_log" and "error_log"
- AWS Key Pair

# Getting Started

The following command will create all needed stack for this demonstration. So you don't need to create anything manually.

```bash
    aws cloudformation create-stack \
        --stack-name amazon-cloudwatch-agent-log-archiving \
        --template-body file://templates/template.yaml \
        --parameters ParameterKey=SSHKey,ParameterValue=<YOUR_SSH_KEY> \
          ParameterKey=BucketName,ParameterValue=<YOUR_BUCKET_NAME> \
        --capabilities CAPABILITY_NAMED_IAM
```

**Values to be replaced:**

**<YOUR_SSH_KEY>** - Select one SSH Key that you previously created.

**<YOUR_BUCKET_NAME>** - Our created bucket for output log files.


**All Right!** We are done and everything is all set. So now you may generate some new logs (accessing the public IPv4 DNS via browser or cURL) to have them in your S3 Bucket. It may take about 5 minutes or when Kinesis Firehoses buffer reach out to 5MB to have the logs in S3.

Copy the output log file from your S3 bucket to your local machine. Check for your respective S3 Key.

```bash
    aws s3 cp s3://<YOUR-BUCKET_NAME>/2021/01/28/16/My-Delivery-Stream-1-2021-01-28-16-37-15-26b143da-347b-4c51-9f5a-8771741e9df5 ./log.gz
```

Unzip the file and open to check the content

```bash
    gunzip log.gz
```

# Cleaning up

Delete all the files inside of the provisioned S3 bucket.

```bash
    aws s3 rm s3://<YOUR_BUCKET_NAME> --recursive
```

Delete the CloudFormation stack.

```bash
    aws cloudformation delete-stack \
	    --stack-name amazon-cloudwatch-agent-log-archiving
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.