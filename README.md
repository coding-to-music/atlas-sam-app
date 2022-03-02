# atlas-sam-app

https://github.com/coding-to-music/atlas-sam-app

### Tasks

- move domain
- use domain
- attach existing S3 buckets
- attach existing SNS topics
- Add more routes
- Better landing page
- get weather or other data
- prevent deleting existing buckets
- connect to free tier for database

This project is a clone of the project: https://github.com/aws/aws-sam-cli-app-templates/tree/master/python3.8/cookiecutter-aws-sam-hello-python superchared with
a MongoDB Atlas deployment!

See [https://github.com/mongodb-developer/get-started-aws-cfn](https://github.com/mongodb-developer/get-started-aws-cfn) to Get Started, or if you know what you're doing
you can create a new SAM python project from this template.

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes [Lambda Powertools for operational best practices](https://github.com/awslabs/aws-lambda-powertools-python), and the following files and folders.

This project will deploy a complete MongoDB Atlas deployment including a new cluster for your python lambda function to connect and write date. There is example code in the hello_world folder to get you started.

- **`hello_world`** - Code for the application's Lambda function.
- **`events`** - Invocation events that you can use to invoke the function.
- **`tests`** - Unit tests for the application code.
- **`template.yaml`** - A template that defines the application's AWS resources.
- **`Makefile`** - Makefile for your convenience to install deps, build, invoke, and deploy your application.

If you prefer to use an integrated development environment (IDE) to build and test your application, you can use the AWS Toolkit.

- [VS Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html)
- [PyCharm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
- [IntelliJ](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
- [Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/welcome.html)

## Requirements

**Make sure you have the following installed before you proceed**

- AWS CLI - [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) configured with Administrator permission
- SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- [Python 3 installed](https://www.python.org/downloads/)
- Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)
- Create an organizational-level [MongoDB Atlas Programmatic API](https://docs.atlas.mongodb.com/configure-api-access#programmatic-api-keys). The key needs `Project Creator` permissions.
- We also recommend [mongocli](https://github.com/mongodb/mongocli) for the easiest way to manage all your MongoDB Atlas needs, cluster and apikeys included!

## Deploy the sample application

> **Already know this sample? Run: `make hurry` - This command will install app deps, build, and deploy your Serverless application using SAM.**

### Deployment Parameters

To deploy your serverless app you will need to supply the following:

| Parameter                  | Required                 | Description                                                                                          | Default                         |
| -------------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------- | ------------------------------- |
| PublicKey                  | Y                        | Your MongoDB Cloud Public API Key                                                                    |
| PrivateKey                 | Y                        | Your MongoDB Cloud Private API Key                                                                   |
| OrgId                      | Y                        | Your MongoDB Cloud Organization Id                                                                   |
| ProjectName                | N                        | The name of the project."                                                                            | `get-started-aws-lambda-python` |
| ClusterName                | N                        | Name of the cluster as it appears in Atlas. Once the cluster is created, its name cannot be changed. | `Cluster-1`                     |
| ClusterInstanceSize        | N                        | Atlas Cluster Tier                                                                                   | `M10`                           |
| ClusterRegion              | N                        | The AWS Region where the Atlas DB Cluster will run. (AWS Region format)                              | `us-east-1`                     |
| ClusterMongoDBMajorVersion | N The version of MongoDB | `latest`                                                                                             |

Build and deploy your application for the first time by running the following commands in your shell:

```bash
atlas-sam-app$ make build
atlas-sam-app$ make deploy.guided
```

The first command will **build** the source of your application within a Docker container. The second command will **package and deploy** your application to AWS. Guided deploy means SAM CLI will ask you about the name of your deployment/stack, AWS Region, and whether you want to save your choices, so that you can use `make deploy` next time.

## Use the SAM CLI to build and test locally

Whenever you change your application code, you'll have to run build command:

```bash
atlas-sam-app$ make build
```

The SAM CLI installs dependencies defined in `hello_world/requirements.txt`, creates a deployment package, and saves it in the `.aws-sam/build` folder.

Test a single function by invoking it directly with a test event:

```bash
atlas-sam-app$ make invoke
```

> An event is a JSON document that represents the input that the function receives from the event source. Test events are included in the `events` folder in this project.

The SAM CLI can also emulate your application's API. Use the `make run` to run the API locally on port 3000.

```bash
atlas-sam-app$ make run
atlas-sam-app$ curl http://localhost:3000/hello
```

The SAM CLI reads the application template to determine the API's routes and the functions that they invoke. The `Events` property on each function's definition includes the route and method for each path.

```yaml
Events:
  HelloWorld:
    Type: Api
    Properties:
      Path: /hello
      Method: get
```

## Fetch, tail, and filter Lambda function logs

To simplify troubleshooting, SAM CLI has a command called `sam logs`. `sam logs` lets you fetch logs generated by your deployed Lambda function from the command line. In addition to printing the logs on the terminal, this command has several nifty features to help you quickly find the bug.

`NOTE`: This command works for all AWS Lambda functions; not just the ones you deploy using SAM.

```bash
atlas-sam-app$ sam logs -n HelloWorldFunction --stack-name <Name-of-your-deployed-stack> --tail
```

You can find more information and examples about filtering Lambda function logs in the [SAM CLI Documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-logging.html).

## Unit tests

Tests are defined in the `tests` folder in this project, and we use Pytest as the test runner for this sample project.

Make sure you install dev dependencies before you run tests with `make dev`:

```bash
atlas-sam-app$ make dev
atlas-sam-app$ make test
```

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
atlas-sam-app$ aws cloudformation delete-stack --stack-name atlas-sam-app
```

# Appendix

## Powertools

**Tracing**

[Tracer utility](https://awslabs.github.io/aws-lambda-powertools-python/core/tracer/) patches known libraries, and trace the execution of this sample code including the response and exceptions as tracing metadata - You can visualize them in AWS X-Ray.

**Logger**

[Logger utility](https://awslabs.github.io/aws-lambda-powertools-python/core/logger/) creates an opinionated application Logger with structured logging as the output, dynamically samples 10% of your logs in DEBUG mode for concurrent invocations, log incoming events as your function is invoked, and injects key information from Lambda context object into your Logger - You can visualize them in Amazon CloudWatch Logs.

**Metrics**

[Metrics utility](https://awslabs.github.io/aws-lambda-powertools-python/core/metrics/) captures cold start metric of your Lambda invocation, and could add additional metrics to help you understand your application KPIs - You can visualize them in Amazon CloudWatch.

## Makefile

We included a `Makefile` for your convenience - You can find all commands you can use by running `make`. Under the hood, we're using SAM CLI commands to run these common tasks:

- **`make build`**: `sam build --use-container`
- **`make deploy.guided`**: `sam deploy --guided`
- **`make invoke`**: `sam local invoke HelloWorldFunction --event events/hello_world_event.json`
- **`make run`**: `sam local start-api`

## Sync project with function dependencies

Pipenv takes care of isolating dev dependencies and app dependencies. As SAM CLI requires a `requirements.txt` file, you'd need to generate one if new app dependencies have been added:

```bash
atlas-sam-app$ pipenv lock -r > hello_world/requirements.txt
```

# My deployment

### Cookiecutter SAM for MongoDB Atlas with Python Lambda functions

https://github.com/mongodb/cookiecutter-aws-sam-python-mongodb-atlas

This ran in AWS for 15 minutes.

```java
sam init --location git@github.com:mongodb/cookiecutter-aws-sam-python-mongodb-atlas.git
```

That created the directory `atlast-sam-app`

```java
curl https://raw.githubusercontent.com/mongodb-developer/get-started-aws-cfn/main/get-setup.sh | bash -s us-east-1
```

```java
sam init --location git@github.com:mongodb/cookiecutter-aws-sam-python-mongodb-atlas.git
project_name [atlas-sam-app]:
runtime [python3.7]:

 +-+-+-+-+-+-+-+-+-+-+-+-+-+
 |M|o|n|g|o|D|B|-|A|t|l|a|s|
 +-+-+-+-+-+-+-+-+-+-+-+-+-+

Hello tmc!
Welcome to your MongoDB Atlas Project: atlas-sam-app
Next steps: (you can cut and paste this snippet)

--------------------------------------------------------------------------

cd atlas-sam-app
sam build
sam deploy --guided --parameter-overrides $(./export-mongocli-parameters.py)

--------------------------------------------------------------------------
```

```java
Initiating deployment
=====================
File with same data already exists at atlas-sam-app/8c186d94ba289583a9cb011c46efc3e0.template, skipping upload

Waiting for changeset to be created..

CloudFormation stack changeset
-----------------------------------------------------------------------------------------------------------------
Operation                    LogicalResourceId            ResourceType                 Replacement
-----------------------------------------------------------------------------------------------------------------
+ Add                        AtlasCluster                 MongoDB::Atlas::Cluster      N/A
+ Add                        AtlasDatabaseUser            MongoDB::Atlas::DatabaseUs   N/A
                                                          er
+ Add                        AtlasProjectIPAccessList     MongoDB::Atlas::ProjectIpA   N/A
                                                          ccessList
+ Add                        HelloWorldFunctionHelloWor   AWS::Lambda::Permission      N/A
                             ldPermissionProd
+ Add                        HelloWorldFunction           AWS::Lambda::Function        N/A
+ Add                        ServerlessRestApiDeploymen   AWS::ApiGateway::Deploymen   N/A
                             t47fc2d5f9d                  t
+ Add                        ServerlessRestApiProdStage   AWS::ApiGateway::Stage       N/A
+ Add                        ServerlessRestApi            AWS::ApiGateway::RestApi     N/A
* Modify                     AtlasProject                 MongoDB::Atlas::Project      True
-----------------------------------------------------------------------------------------------------------------

Changeset created successfully. arn:aws:cloudformation:us-east-1:708090526287:changeSet/samcli-deploy1646187846/3e6af1fd-1c1c-4513-879b-580d7d768755


Previewing CloudFormation changeset before deployment
======================================================
Deploy this changeset? [y/N]: y

2022-03-02 02:24:20 - Waiting for stack create/update to complete

CloudFormation events from stack operations
-----------------------------------------------------------------------------------------------------------------
ResourceStatus               ResourceType                 LogicalResourceId            ResourceStatusReason
-----------------------------------------------------------------------------------------------------------------
DELETE_COMPLETE              MongoDB::Atlas::Project      AtlasProject                 Delete succeeded for the
                                                                                       resources that failed to
                                                                                       create.
CREATE_IN_PROGRESS           MongoDB::Atlas::Project      AtlasProject                 -
CREATE_IN_PROGRESS           MongoDB::Atlas::Project      AtlasProject                 Resource creation
                                                                                       Initiated
CREATE_COMPLETE              MongoDB::Atlas::Project      AtlasProject                 -
CREATE_IN_PROGRESS           MongoDB::Atlas::DatabaseUs   AtlasDatabaseUser            -
                             er
CREATE_IN_PROGRESS           MongoDB::Atlas::Cluster      AtlasCluster                 -
CREATE_IN_PROGRESS           MongoDB::Atlas::ProjectIpA   AtlasProjectIPAccessList     -
                             ccessList
CREATE_IN_PROGRESS           MongoDB::Atlas::ProjectIpA   AtlasProjectIPAccessList     Resource creation
                             ccessList                                                 Initiated
CREATE_IN_PROGRESS           MongoDB::Atlas::DatabaseUs   AtlasDatabaseUser            Resource creation
                             er                                                        Initiated
CREATE_IN_PROGRESS           MongoDB::Atlas::Cluster      AtlasCluster                 Resource creation
                                                                                       Initiated
CREATE_COMPLETE              MongoDB::Atlas::ProjectIpA   AtlasProjectIPAccessList     -
                             ccessList
CREATE_COMPLETE              MongoDB::Atlas::DatabaseUs   AtlasDatabaseUser            -
                             er
CREATE_COMPLETE              MongoDB::Atlas::Cluster      AtlasCluster                 -
CREATE_IN_PROGRESS           AWS::Lambda::Function        HelloWorldFunction           -
CREATE_IN_PROGRESS           AWS::Lambda::Function        HelloWorldFunction           Resource creation
                                                                                       Initiated
CREATE_COMPLETE              AWS::Lambda::Function        HelloWorldFunction           -
CREATE_IN_PROGRESS           AWS::ApiGateway::RestApi     ServerlessRestApi            -
CREATE_IN_PROGRESS           AWS::ApiGateway::RestApi     ServerlessRestApi            Resource creation
                                                                                       Initiated
CREATE_COMPLETE              AWS::ApiGateway::RestApi     ServerlessRestApi            -
CREATE_IN_PROGRESS           AWS::ApiGateway::Deploymen   ServerlessRestApiDeploymen   -
                             t                            t47fc2d5f9d
CREATE_IN_PROGRESS           AWS::Lambda::Permission      HelloWorldFunctionHelloWor   -
                                                          ldPermissionProd
CREATE_IN_PROGRESS           AWS::Lambda::Permission      HelloWorldFunctionHelloWor   Resource creation
                                                          ldPermissionProd             Initiated
CREATE_COMPLETE              AWS::ApiGateway::Deploymen   ServerlessRestApiDeploymen   -
                             t                            t47fc2d5f9d
CREATE_IN_PROGRESS           AWS::ApiGateway::Deploymen   ServerlessRestApiDeploymen   Resource creation
                             t                            t47fc2d5f9d                  Initiated
CREATE_IN_PROGRESS           AWS::ApiGateway::Stage       ServerlessRestApiProdStage   -
CREATE_IN_PROGRESS           AWS::ApiGateway::Stage       ServerlessRestApiProdStage   Resource creation
                                                                                       Initiated
CREATE_COMPLETE              AWS::ApiGateway::Stage       ServerlessRestApiProdStage   -
CREATE_COMPLETE              AWS::Lambda::Permission      HelloWorldFunctionHelloWor   -
                                                          ldPermissionProd
UPDATE_COMPLETE_CLEANUP_IN   AWS::CloudFormation::Stack   atlas-sam-app                -
_PROGRESS
UPDATE_COMPLETE              AWS::CloudFormation::Stack   atlas-sam-app                -
-----------------------------------------------------------------------------------------------------------------

CloudFormation outputs from deployed stack
------------------------------------------------------------------------------------------------------------------
Outputs
------------------------------------------------------------------------------------------------------------------
Key                 HelloWorldFunctionIamRole
Description         Implicit IAM Role created for Hello World function
Value               arn:aws:iam::708090526287:role/atlas-sam-app-HelloWorldFunctionRole-2ZEQ6FOMJ74G

Key                 HelloWorldApi
Description         API Gateway endpoint URL for Prod stage for Hello World function
Value               https://3ng095f25d.execute-api.us-east-1.amazonaws.com/Prod/hello/

Key                 HelloWorldFunction
Description         Hello World Lambda Function ARN
Value               arn:aws:lambda:us-east-1:708090526287:function:atlas-sam-app-HelloWorldFunction-yS6LMilvTn2r
------------------------------------------------------------------------------------------------------------------

Successfully created/updated stack - atlas-sam-app in us-east-1
```

# Cleaning Up

```java
aws cloudformation list-stacks
```

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
atlas-sam-app$ aws cloudformation delete-stack --stack-name atlas-sam-app
```

## AWS IAM permissions

From https://github.com/mongodb-developer/get-started-aws-cfn/blob/main/README.md

May not be required for this project

This project requires various kinds of AWS permissions.

To run the setup step (`[get-setup.sh](./get-setup.sh)`) AWS team account owners should be able to assume the [policy-quickstart-mongodb-atlas-installer.yaml](./policy-quickstart-mongodb-atlas-installer.yaml) sample permission set. This step requires above average permissions.

For the started step (`[get-started.sh](./get-started.sh)`) AWS users should be able to assume the [policy-quickstart-mongodb-atlas-user.yaml](./policy-quickstart-mongodb-atlas-user.yaml) sample permission set.

We've included a sample minimal example set of policies, which you can assume safely and use with this project.

Create new AWS IAM roles with the supplied policies. Below are examples on how to do that via AWS CloudFormation.
You can then assume the role with `aws sts assume-role`. We recommend this for exporting your AWS environment into the Docker environment to run this project (note the `--role-arn` used in step 2 is created in the step 1).

#### AWS::IAM::Role MongoDB-Atlas-CloudFormation-Installer

```
aws cloudformation create-stack \
  --capabilities CAPABILITY_NAMED_IAM \
  --template-body file://./policy-quickstart-mongodb-atlas-installer.yaml \
  --stack-name quickstart-mongodb-atlas-installer-role
```

```
source <(aws sts assume-role --role-arn arn:aws:iam::<YOUR_AWS_ACCOUNT>:role/MongoDB-Atlas-CloudFormation-Installer --role-session-name "get-started-installer"  | jq -r  '.Credentials | @sh "export AWS_SESSION_TOKEN=\(.SessionToken)\nexport AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey) "')
```

#### AWS::IAM::Role MongoDB-Atlas-CloudFormation-User

```
aws cloudformation create-stack \
  --capabilities CAPABILITY_NAMED_IAM \
  --template-body file://./policy-quickstart-mongodb-atlas-user.yaml \
  --stack-name quickstart-mongodb-atlas-user-role
```

```
source <(aws sts assume-role --role-arn arn:aws:iam::<YOUR_AWS_ACCOUNT>:role/MongoDB-Atlas-CloudFormation-User --role-session-name "get-started-user"  | jq -r  '.Credentials | @sh "export AWS_SESSION_TOKEN=\(.SessionToken)\nexport AWS_ACCESS_KEY_ID=\(.AccessKeyId)\nexport AWS_SECRET_ACCESS_KEY=\(.SecretAccessKey) "')
```

Remember to UNSET your `AWS_*` environment variables to "un-assume" the role.

```
source <(env | awk -F= '/AWS/ {print "unset ", $1}'
```

(Reference: [get-token.md](https://gist.github.com/brianredbeard/035ee1419bc38a0e2d854fb828d585d7))

<sup>
<a id="qs-repo">[1]</a> Access coming soon: [AWS Quick Start for MongoDB Atlas](https://github.com/aws-quickstart/quickstart-mongodb-atlas)
</sup>

<sup>
<a id="qs-res-repo">[2]</a> Access coming soon: [MongoDB Atlas CloudFormation resources](https://github.com/aws-quickstart/quickstart-mongodb-atlas-resources)
</sup>
