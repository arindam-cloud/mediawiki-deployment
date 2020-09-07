# Details

This is an assignment to deploy a Mediawiki stack. For this, we have used a Autoscaling group for the Web Servers and RDS for the DB instance with an option for Multi-AZ.

You'll find the below files and functions related to the automated deployment process -

#### mediawiki-deployment.yaml

This is the main Cloudformation template to deploy the stack. This can be run with multiple parameters according to the environment.

#### dev-stack-configuration.json & prod-stack-configuration.json
These are the files which store the parameter values in a key-pair format. These can be modified according to need and before deploying the stack. The configuration files specify the parameter values that your pipeline uses when it creates the dev and production stacks.

#### codepipeline-deployment.yaml
This is the Cloudformation template to build the AWS Codepipeline to fetch the details from source and then automate the deployment. There will be 3 stages -
a. Source
b. Dev stage
c. Prod stage

The mediawiki-deployment.yaml template gets passed as a parameter for the Codepipeline template, along with the environment configuration files.

The pipeline also has an Amazon SNS task to notify the admin user via mail when the Dev stage is complete, and only once admin approves this will move towards the Prod deploy stage.

#### codepipeline-parameters.json
This file needs to be updated with the parameter values for the Codepipeline template

## Deployment Steps

Please find the steps below to run the automation -

###### Step 1 - Sign in to your AWS account and navigate to Cloudformation. Please choose a region where you have existing keypair available.

###### Step 2 - Modify the parameter files - dev-stack-configuration.json and prod-stack-configuration.json according to the values for your region.

###### Step 3 - Once the parameters have been updated, create a zip file using mediawiki-deployment.yaml, dev-stack-configuration.json & prod-stack-configuration.json

###### Step 4 - Navigate to S3 under your AWS account and create a bucket (for ex - my-template-repo) and upload the zip file under that bucket.

###### Step 5 - Modify the codepipeline-parameters.json with values for parameters for the Codepipeline. Values for TemplateFileName, TestStackConfig and ProdStackConfig are updated to the respective file name by default.

###### Step 6 - Use the below AWS CLI command to provision the codepipeline-deployment.yaml template. Please make sure that you are creating the pipeline in the region as your S3 bucket -
aws cloudformation create-stack --stack-name cp --template-body file://codepipeline-deployment.yml --parameters file://codepipeline-parameters.json

###### Step 7 - Navigate to the CodePipeline console to check the Pipeline steps and progress.

You can find the Mediawiki stack Output URL using the below command -

###### aws cloudformation --region your-region describe-stacks --stack-name your-stackname --query "Stacks[0].Outputs[?OutputKey=='WebsiteURL'].OutputValue" --output text