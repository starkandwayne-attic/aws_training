AWS training tools
==================

This project is a collection of scripts used by Stark & Wayne to conduct training courses using AWS.

Requirements
------------

-	Manually create more AWS accounts than you'll need
-	Store the AWS accounts details in an Excel/Google Spreadsheet and export to a CSV file
	-	Email, master password, master API keys
-	OS X/Linux (currently helper scripts written in Bash)
	-	Bash
	-	Ruby 1.9+ (Ruby 2.1+ preferred)
	-	RubyGems
	-	[Terraform](https://www.terraform.io/) ([download](https://www.terraform.io/downloads.html)\)

Setup
-----

```
git clone https://github.com/starkandwayne/aws_training.git
cd aws_training
bundle install
```

Structure
---------

You need to create the following:

-	`accounts/students.csv` - the exported CSV file containing the master AWS credentials for each account
-	`accounts/students.yml` - the YAML file containing AWS credentials for all the AWS accounts
-	`accounts/signin_urls.yml` - the YAML file containing the signin URL for the IAM student users

Usage
-----

To create all the student IAM accounts:

```
aws_student_accounts create-students -C accounts/students.yml
```

To verify all the student IAM credentials for their own AWS account (and observe # instances):

```
aws_student_accounts verify-credentials -C accounts/students.yml
```

After training course completed
-------------------------------

To clean out all the instances & IP address from all accounts, though does not delete the student's IAM credentials:

```
aws_student_accounts clean-accounts -C accounts/students.yml
```

To delete the IAM student credentials (after a training course), though does not delete any instances/IP addresses:

```
aws_student_accounts delete-students -C accounts/students.yml
```
