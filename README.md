


# CI-CD pipeline using AWS Developer Tools ( CodeBuild, CodeDeploy and and CodePipeline) 
###### //https://s3.amazonaws.com/stelligent-public/cloudformation-templates/github/labs/codebuild/codebuild-cpl-cd-cc.json

# Steps:
## 1.	create an S3 artifact bucket 
Create a bucket in ***us-east-1*** with  syntax `codepipeline-<region-code>-<accountID>`, e.g codepipeline-us-east-1-5678543245
## 2.	Configure Credentials:
Generate a GitHub Token. This token will be used by AWS pipeline to authenticate to GitHub. 

### How to Generate a GitHub Personal Access Token (PAT)

For public GitHub repositories, AWS CodePipeline still requires an OAuth token for authentication. This token is used for securely accessing GitHub, even if the repository is public.

A Personal Access Token (PAT) allows AWS CodePipeline to authenticate and access your GitHub repositories, even public ones.

#### 1. Log into GitHub
#### 2. Navigate to Developer Settings
- Click on your profile picture (top-right corner).
- Go to Settings → Scroll down → Developer settings.

#### 3. Generate a New Token
- In the sidebar, select Personal access tokens → Tokens (classic).
- Click on Generate new token → Generate new token (classic).

#### 4. Configure the Token
- Note: For public repositories, minimal scopes are required.
- Token Name: CodePipelineToken (or any name you prefer)
- Expiration: Choose an expiration date (e.g., 90 days).
- Scopes (Permissions): Select the following:
- ✅ repo → Full control of private repositories (optional for public repos)
- ✅ admin:repo_hook → Manage repository hooks (important for triggering builds)

#### 5. Generate and Copy the Token
- Click Generate token.
- Important: Copy the token immediately. GitHub won’t show it again.


### Store the Token in AWS Secrets Manager
- Open AWS Console → Secrets Manager.
- Click Store a new secret.
- Select Other type of secret.
- Key: OAuthToken → Value: Paste the GitHub token here.
- Secret Name: GitHubToken.
- Click Next → Store.


- Confirm Permissions
- Ensure CodePipeline has permission to access Secrets Manager:
- Sample  Policy:
	{
 	 "Effect": "Allow",
 	 "Action": [
  	  "secretsmanager:GetSecretValue"
  	],
  	"Resource": "*"
	}

AWS CodePipeline should now be able to authenticate with GitHub using your token stored in Secrets Manager.

## 3  Fork Sample Application repository into your GitHub space
- Fork this repository into your GitHub space

## 4  Deploy Cloud Formation Template and configure necessary parameters.
- Create a keypair --> to be used by the stack. This template has a `nested CodeDeployEC2 Instance Stack` that creates and configure and EC2 for deployment. 
- Copy the cloudformation template [here](https://github.com/mecbob/AWS-DevTools-GitHub/blob/main/aws-developerTools-GitHub.json) and deploy in ***us-east-1***

