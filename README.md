# Terraform Cloud

 Terraform Cloud is a managed service that provides you with Terraform CLI to provision infrastructure, either on demand or in response to various events.

By default, Terraform CLI performs operation on the server whene it is invoked, it is perfectly fine if you have a dedicated role who can launch it, but if you have a team who works with Terraform – you need a consistent remote environment with remote workflow and shared state to run Terraform commands.

Terraform Cloud executes Terraform commands on disposable virtual machines, this remote execution is also called remote operations.

## Migrate Terraform files to Terraform Cloud
-  Create a Terraform Cloud account from this link https://app.terraform.io/signup/account
- Create an Organization. Click "Start from Scratch", enter a name and click create.
- Create a workspace.
  - Create a new GitHub repo that will contain your tf configuration
  - Push the files from the previous projects to the repo
  - On Terraform Cloud, click create workspace
  - Select "version control workflow"

![{3221FEAE-65E8-46FF-B4EC-D6270C715623} png](https://user-images.githubusercontent.com/76074379/132108591-e8b10df6-bf44-4fb8-83c7-153e35af2041.jpg)

  - Add the newly created repo
  - Provide a description for your workspace and click "Create workspace"
  
  ![{41E5698F-CC07-41C2-923F-D9D9827EA6CF} png](https://user-images.githubusercontent.com/76074379/132105461-ff101bf8-4dd7-4bfa-89cf-39b8b2d865c5.jpg)
  
- Configure variables
  - Click Variables from the top tab in your workspace, under the workspace name
  - Scroll down to Environment Variables and create variables for your access key id and secret access key you got from AWS.
  
  ![{089F0B80-1BA9-4163-987F-BA17B7AB025C} png](https://user-images.githubusercontent.com/76074379/132105503-dbdb84d5-6fec-4866-b3cb-ce7889ce7d22.jpg)
  
- Run **plan** and **apply** from console
  - Click on the Runs tab
  - Click "Queue plan"
  - After the **plan** run is complete, click "Confirm and apply". Enter a comment and click "Confirm plan" to apply the configuration.
  
![{30FE31FC-0A18-49C3-B465-2A5F2354BC77} png](https://user-images.githubusercontent.com/76074379/132105581-e959d182-0b4c-4060-a4ec-497a49c51312.jpg)

![{7F9AF193-D0C2-47E7-9E2B-BDC5725BCDBA} png](https://user-images.githubusercontent.com/76074379/132105620-ff7b1cd3-a213-46f5-82b2-f0b70d87e4c5.jpg)

- Test automated **terraform plan** 
  - Edit a file in your repo, and commit. A **plan** run should be triggered automatically.

## Practice Task 1
- Configure 3 branches in your terraform repo for dev, test and prod
- Make necessary configurations to trigger runs automatically only for **dev** environment.
  - Create a new workspace, select "version control workflow"
  - Select "GitHub" as version control provider
![{1E25EF58-0C5D-4CAE-B2A2-F474E0E81D2B} png](https://user-images.githubusercontent.com/76074379/132105912-2ff48417-0a6d-46c6-9670-b74819b6c90b.jpg)

  - Choose the repo that contains your tf files
  - Enter the workspace name (e.g terraform-cloud-dev)
  - Click Advanced options, under VCS branch, enter the branch you want to configure (e.g dev)
  - Click Create Workspace
![Inked{8823D0FA-820C-4C9D-85A7-C49C205805A2} png_LI](https://user-images.githubusercontent.com/76074379/132108186-0bc7dd7d-4f83-45e5-bd0a-e354b19961dd.jpg)

  - Configure your variables

![{23356B2F-5A66-41A8-966A-9A7C9F39F54B} png](https://user-images.githubusercontent.com/76074379/132106515-8719c118-46ce-4d62-949f-4d6ef8a20a6f.jpg)

  - Click the Settings drop down, beside Variables
  - Click Run Triggers
  - Select the workspace you created previously
- Create email and Slack notifications
  - Click on Settings -> Notifications
  - Select Email
  - Enter a name for the notification
  - Select notfication recipients
  - Under Triggers, click the "Only certain events" radio button
  - Check the boxes you want to be notfied for

![{42D15FCE-73EF-442C-96CF-DC0DA112F43C} png](https://user-images.githubusercontent.com/76074379/132106226-b5366b95-a26d-428f-a3f1-290e65dc596c.jpg)
  - To configure Slack notification, choose Slack instead of Email
  - See how to get your Slack webhook here: https://api.slack.com/messaging/webhooks#create_a_webhook
  - Then configure everything else as with Email configuration
- Apply destroy
  - Click Settings -> "Destruction and Deletion"
  - Under Manually destroy, click "Queue destroy plan"
  - Enter the workspace name and click "Queue destroy plan"

![{BD4E99F1-5DEA-41FC-A465-97E5EABF5D33} png](https://user-images.githubusercontent.com/76074379/132106665-0b67d920-2511-4ada-b6bd-739f45657088.jpg)

## Practice Task 2 (https://learn.hashicorp.com/tutorials/terraform/module-private-registry-share)
- Create a simple Terraform repository that will be your module
  - Clone from this repo https://github.com/hashicorp/learn-private-module-aws-s3-webapp
  - Your repo's name should be in the form terraform-\<PROVIDER>-\<NAME>
  - Click "Tag release" from the left pane
  - Create a new release
  - Click "Create a new release", and add 1.0.0 to the tag version field, and set the Release title to anything you like.
  - Click "Publish release"

![{7AD3A1AB-B7FE-44E9-949B-9AF8965590CC} png](https://user-images.githubusercontent.com/76074379/132107006-d23c3313-a300-4d62-9f5e-14e84d808a9a.jpg)

- Import the module into your private repository
  - On your Terraform cloud, click Registry on the top pane
  - You'll need to add a VCS provider. Select GitHub (Custom) when prompted
  - Follow the outlined steps 
  - Click connect and continue 

![Inked{B75F92F8-8A4B-419E-B368-2AC2E4A89345} png_LI](https://user-images.githubusercontent.com/76074379/132107117-1d57d5d9-8bc7-4cc9-97dd-03f7e6d57798.jpg)

  - Go back to Registry
  - Click "Publish private module"
  - Click the VCS you configured and find the name of your module repo
  - Select the module and click the "Publish module" button
  - Copy the configuration details, you'll need it later for when you want to use the module
- Create a configuration that uses the module
  - you can fork this repo https://github.com/hashicorp/learn-private-module-root/
  - Create main.tf, variables.tf and outputs.tf files
  - In your main.tf file, paste in the following block
  ```
  terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
    }
  }
  }

  provider "aws" {
    region = var.region
  }

  module "s3-webapp" {
    source  = "app.terraform.io/Darey-PBL/s3-webapp/aws"
    name   = var.name
    region = var.region
    prefix = var.prefix
    version = "1.0.0"
  }
  ```
  ![{3DA80531-0AB9-404F-902B-2CFB7BADB882} png](https://user-images.githubusercontent.com/76074379/132107226-0824d01d-8782-4a00-b372-c5e6a2aba978.jpg)
  
  replace the **module** block with the configuration details you copied earlier.
  - In your variables.tf file, add the following
  ```
  variable "region" {
    description = "This is the cloud hosting region where your webapp will be deployed."
  }

  variable "prefix" {
    description = "This is the environment your webapp will be prefixed with. dev, qa, or prod"
  }

  variable "name" {
    description = "Your name to attach to the webapp address"
  }
  ```
  
- Terraform Cloud uses the outputs.tf file to display your module outputs as you run them in the web UI.
 ```
  output "website_endpoint" {
  value = module.s3-webapp.endpoint
}
```
## Create a workspace for the configuration
In Terraform Cloud, create a new workspace and choose your GitHub connection.

Terraform Cloud will display a list of your GitHub repositories. You may need to filter by name to find and choose the your root configuration repository, called learn-private-module-root.

Leave the workspace name and "Advanced options" unchanged, and click the purple "Create workspace" button to create the workspace.

Once your configuration is uploaded successfully, choose "Configure variables."

You will need to add the three Terraform variables prefix, region, and name. These variables correspond to the variables.tf file in your root module configuration and are necessary to create a unique S3 bucket name for your webapp. Add your AWS credentials as two environment variables, AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY and mark them as sensitive.

![{92E2D916-F4D6-4222-93AA-43D58B17D539} png](https://user-images.githubusercontent.com/76074379/132107624-9a62b4d9-02a9-4f72-9db7-b13f559f4261.jpg)

## Deploy the infrastructure

Test your deployment by queuing a plan, then confriming and applying the plan in your Terraform Cloud UI.

![{9123759B-4144-461F-8F83-248FA6AB30B3} png](https://user-images.githubusercontent.com/76074379/132107732-f8ebd9be-aab8-4298-868d-a568eaef3525.jpg)

![{074CD6D3-90F4-472D-95AE-9CB8FDC67061} png](https://user-images.githubusercontent.com/76074379/132107757-70919cc7-48d5-466b-a17b-f47a37fdb265.jpg)


## Destroy the deployment

You will need queue a destroy plan in the Terraform Cloud UI for your workspace, by clicking the "Queue destroy plan" button.

![{5F6A230B-E0CD-4435-9617-7D474C28F447} png](https://user-images.githubusercontent.com/76074379/132109240-5e9f941b-9eb6-49af-a6d7-23205dc79faa.jpg)

