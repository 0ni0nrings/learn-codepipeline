# learn-codepipeline

Labs helping you to learn AWS CodePipeline within a day. Labs are based on the [AWS Velocity Series](https://cloudonaut.io/aws-velocity-series/).

Are you looking for an instructor-led workshop based on these labs? Say [hello@widdix.net](mailto:hello@widdix.net).

## Labs

* [Lab 01: Deploying a CloudFormation template](lab01-cloudformation/)
* [Lab 02: Building with CodeBuild](lab02-codebuild/)
* [Lab 03: Deploying to Lambda and API Gateway (Serverless)](lab03-serverless/)
* [Lab 04: Deploying to ECS](lab04-ecs/)

## Setup your personal lab environment

Clone this repository on your local machine.

```
git clone https://github.com/widdix/learn-codepipeline.git
```

Create a CodeCommit repository. Replace `$user` with your name (e.g. `andreas`).

```
aws codecommit create-repository --repository-name learn-codepipeline-$user
```

The command will return some information about your CodeCommit repository.

```
{
    "repositoryMetadata": {
        "accountId": "111111111111",
        "repositoryId": "c1998fd8-2c78-4a73-984c-53d588a6b237",
        "repositoryName": "learn-codepipeline-andreas",
        "lastModifiedDate": 1541665280.947,
        "creationDate": 1541665280.947,
        "cloneUrlHttp": "https://git-codecommit.eu-west-1.amazonaws.com/v1/repos/learn-codepipeline-andreas",
        "cloneUrlSsh": "ssh://git-codecommit.eu-west-1.amazonaws.com/v1/repos/learn-codepipeline-andreas",
        "Arn": "arn:aws:codecommit:eu-west-1:111111111111:learn-codepipeline-andreas"
    }
}
```

Execute the following command to add a new remote named `deploy` to your cloned repository. Replace `$cloneUrlHttp` with the output from the previous command.

```
git remote add deploy $cloneUrlHttp
```

Edit `.git/config` and add.

```
[credential]
    helper =
    helper = !aws codecommit credential-helper $@
    UseHttpPath = true
```

```
git push deploy master
```

## Clean up

Make sure you are deleting all the resources created while going through the labs.

* Delete the CodeCommit repository `learn-codepipeline-$user`.
* Got to the CloudFormation stack `learn-codepipeline-$user` and note down the name of the S3 bucket with the logical ID `ArtifactsBucket`.
* Delete the CloudFormation stack `learn-codepipeline-$user`.
* Delete the S3 bucket you noted down before deleting the CloudFormation stack.

## Setup the global infrastructure for all labs

> Setup the global infrastructure only once for all participants of the workshop.

Adjust cluster size according to number of workshop participants.

```
aws cloudformation create-stack --stack-name vpc-2azs --template-url https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/stable/vpc/vpc-2azs.yaml

aws cloudformation create-stack --stack-name ecs-cluster --template-url https://s3-eu-west-1.amazonaws.com/widdix-aws-cf-templates-releases-eu-west-1/stable/ecs/cluster.yaml --parameters ParameterKey=ParentVPCStack,ParameterValue=vpc-2azs ParameterKey=InstanceType,ParameterValue=m5.large ParameterKey=MaxSize,ParameterValue=2 ParameterKey=MinSize,ParameterValue=2, ParameterKey=DesiredCapacity,ParameterValue=2 --capabilities CAPABILITY_IAM
```

## More Labs

We offer AWS workshops tailored to your needs. See [widdix/learn-*](https://github.com/widdix?q=learn-) for more labs.
