#### Terraform AWS S3 Static Website Hosting 

Hello Everyone .....this is an step by step  process of hosting a static website on AWS S3 using Terraform. You can find the complete project files in this repository.

## Project Overview
Using Terraform, we will automate the deployment of a static website on AWS S3. This project includes:

- Creating an S3 bucket
- Uploading an index page and a custom error page
- Configuring the bucket for static website hosting
- Outputting the website endpoint

#### Project Files
1. `index.html`
2. `error.html`
3. `variables.tf`
4. `output.tf`
5. `main.tf`
6. `provider.tf`

## Prerequisites
Before you begin, ensure you have the following:

1. **AWS Account**: You need an active AWS account to create S3 buckets and other resources.
2. **Terraform Installed**: Install Terraform on your local machine. Follow the instructions on the [Terraform website](https://learn.hashicorp.com/tutorials/terraform/install-cli).
3. **AWS CLI Configured**: Configure AWS CLI with your credentials to allow Terraform to interact with your AWS account. You can configure it using `aws configure` command.
4. **Basic HTML Knowledge**: Understanding of basic HTML to create the website's index and error pages.

## Step-by-Step Procedure

### 1. Create HTML Files
Create two HTML files in your project directory:
- `index.html`: The main page of your website.
- `error.html`: The error page displayed if there is an error.

### 2. Set Up Terraform Configuration

#### variables.tf
Define the S3 bucket name.
```hcl
variable "bucketname" {
  default = "testtfpareshbucket"
}
```

#### provider.tf
Configure the AWS provider.
```hcl
terraform {
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "5.58.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}
```

#### main.tf
Create the S3 bucket and upload the HTML files.
```hcl
resource "aws_s3_bucket" "bucket1" {
  bucket = var.bucketname

  website {
    index_document = "index.html"
    error_document = "error.html"
  }
}

resource "aws_s3_bucket_object" "index" {
  bucket = aws_s3_bucket.bucket1.bucket
  key    = "index.html"
  source = "index.html"
  acl    = "public-read"
}

resource "aws_s3_bucket_object" "error" {
  bucket = aws_s3_bucket.bucket1.bucket
  key    = "error.html"
  source = "error.html"
  acl    = "public-read"
}

resource "aws_s3_bucket_public_access_block" "public_access_block" {
  bucket = aws_s3_bucket.bucket1.id

  block_public_acls   = false
  block_public_policy = false
  ignore_public_acls  = false
  restrict_public_buckets = false
}
```

#### output.tf
Output the website endpoint.
```hcl
output "websiteendpoint" {
  value = aws_s3_bucket.bucket1.website_endpoint
}
```

### 3. Deploying with Terraform

#### Initialize Terraform
Run the following command to initialize Terraform:
```sh
terraform init
```

#### Validate Configuration
Validate the configuration to check for any errors:
```sh
terraform validate
```

#### Apply Configuration
Apply the configuration to deploy the infrastructure and upload the website files:
```sh
terraform apply
```
Type `yes` to confirm the apply action.

#### Access the Website
After applying the configuration, Terraform will provide the website endpoint. Open it in your browser to see your static site.

## Challenges and Fixes

1. **Reference to Undeclared Resource**:
   - **Issue**: Errors due to references to resources that were not declared.
   - **Fix**: Ensured all resources were correctly named and declared.

2. **Missing Required Argument**:
   - **Issue**: Missing required arguments in the configurations.
   - **Fix**: Added all necessary arguments to the configurations.

3. **Unsupported Argument**:
   - **Issue**: Using arguments that were not supported.
   - **Fix**: Removed unsupported arguments and used the correct syntax.

4. **Duplicate Resource Configuration**:
   - **Issue**: Duplicate resource names causing conflicts.
   - **Fix**: Ensured unique names for all resources to avoid conflicts.

## Conclusion
Completing this project taught me how to automate cloud infrastructure using Terraform. Despite facing some errors, I learned how to troubleshoot and resolve them effectively.

Feel free to reach out if you have any questions or suggestions!

#Terraform #AWS #StaticWebsite #IaC #DevOps
