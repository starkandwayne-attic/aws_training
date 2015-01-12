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

### Create all the student IAM accounts

```
aws_student_accounts create-students -C accounts/students.yml
```

This will create a folder `students` and one subfolder per key in `students.yml` for each AWS account.

To verify all the student IAM credentials for their own AWS account (and observe # instances):

```
aws_student_accounts verify-credentials -C accounts/students.yml
```

To clean out all the instances & IP address from all accounts to ensure they are all empty:

```
aws_student_accounts clean-accounts -C accounts/students.yml
```

### Bootstrap Cloud Foundry via terraform

```
./terraform-aws-cf-install student1
```

Repeat this for each student (sorry, no automation yet).

SUGGESTION: Use `tmux` and create one window per student.

Training helpers
----------------

### SSH into any student's bastion VM

```
./bastion-ssh student1
```

### Interactive REPL for fog for a student

```
./fog-account student1
```

### Target Cloud Foundry CLI at student's CF

```
./cf-login student1
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
