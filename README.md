# Utilize Terraform within a GitHub Action to deploy to AWS

Developed a GitHub Action that establishes a connection using OpenID Connect (OIDC) for secure authentication. This action is configured to execute a Terraform plan when pull requests are submitted, providing a preview of the potential infrastructure changes. Once the pull request is approved and merged into the main branch, the GitHub Action automatically applies the Terraform changes to update the infrastructure on AWS accordingly. This workflow ensures that all modifications are reviewed and tested before deployment, enhancing the overall reliability and security of the infrastructure management process.



![Image](https://github.com/users/myt7ics/projects/1/assets/58075201/06414447-bde6-4140-b5fe-4b3da2400cd7)



## Table of Content

1. [Authentication](#authentication)
2. [Create S3 Bucket](#create-s3-bucket)
3. [Create GitHub Policy](#create-gitHub-policy)
4. [Create AWS Policy](#create-aws-policy)
5. [Add Repository Secrets](#add-repository-secrets)
6. [GitHub Action Setup](#gitHub-action-setup)
7. [Pull Request](#pull-request)
8. [Check GitHub Action](#check-gitHub-action)

 


## Authentication 

- GitHub Action requests a JWT from GitHub OIDC provider.
- GitHub OIDC provider issues a signed JWT to the GitHub Action.
- GitHub Action requests a temporary access token from AWS IAM identity provider using the signed JWT.
- IAM identity provider verifies the signed JWT with GitHub OIDC provider and checks if the role can be assumed.
- IAM identity provider issues a temporary access token to the GitHub Action using the role.
- GitHub Action environment gains access to AWS with the permissions specified in the assumed role.

## Create S3 Bucket 

- Create an S3 bucket to store Terraform state files.
- Enable encryption and select the key from Amazon.
- Create GitHub Role 
- Create an IAM role that GitHub can assume.
- Use a custom trust policy to allow the role to be assumed by the GitHub OIDC provider.
- Modify the policy with your account number, GitHub username, repository name, and optionally specific branches.

## Create GitHub Policy 

- Create an IAM policy to allow the GitHub role to access the S3 bucket where Terraform state files are stored.
- Attach the policy to the GitHub role.

## Create AWS Policy 

- Create an IAM policy for the resources you want to deploy in AWS (e.g., AWS SSM parameters).
- Attach the policy to the GitHub role.
- Add Terraform Code to GitHub 
- Copied the Arn for later use.
- Added Terraform code to the GitHub repository.
- Created an SSM parameter in AWS.
- Added the provider block to the Terraform code.
- Committed the changes to the GitHub repository.

## Add Repository Secrets 

- Added secrets to the repository:
- AWS bucket name
- AWS bucket keyname
- AWS role

## GitHub Action Setup

- GitHub action is triggered on a push to the main branch or a pull request.
- Set up permissions for the AWS OIDC connection, checkout action, and GitHub bot.
- GitHub action clones the repository and has access to the files.
- Used the official AWS action to configure credentials through OIDC connection.
- Used the official HashiCorp action to install Terraform on the GitHub action.
- Performed Terraform format, init, validation, and plan.
- Used the GitHub script action to post information about the plan to the pull request.
- Performed Terraform apply only on a push directly to the main branch.

## Pull Request 

- GitHub action is running.
- Terraform plan is successful.

## Check GitHub Action 

- GitHub action is successful.
- Verified that the Terraform code executed correctly by checking the SSM parameter store.
