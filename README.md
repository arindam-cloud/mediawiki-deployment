# Deployment Steps

This is a Cloudformation Template to automate deployment of the Mediawiki stack.

Please find the steps below to run the stack -

### Step 1 - Sign in to your AWS account and navigate to Cloudformation. Please choose a region where you have existing keypair available.

### Step 2 - Select the option "Create Stack". Provide values for parameters and hit "Create Stack"

## Alternative option -

If you have AWS CLI installed on your local machine, you can directly use the following command to create the stack -

### Step 1 - Clone the Git repo and edit the parameters.json file with your customized values

### Step 2 - Run the CLI command:

### aws cloudformation create-stack --stack-name yourstackname --template-body file://mediawiki-deployment.yaml --parameters file://parameters.json

You can find the output URL using the following command:

### aws cloudformation --region ap-southeast-2 describe-stacks --stack-name yourstackname --query "Stacks[0].Outputs[?OutputKey=='WebsiteURL'].OutputValue" --output text