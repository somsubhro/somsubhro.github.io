---
permalink: /struggles.html
---
[Home](/README.md) | [Struggles](/struggles.md)

## S3 bucket names

Just making sure, that we all understand this that:👇 👇

*"An Amazon S3 bucket name is globally unique, and the namespace is shared by **all AWS accounts**. This means that after a bucket is created, the name of that bucket cannot be used by another AWS account in any AWS Region until the bucket is deleted"* - From [this Buckets overview](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html)

This means `trial-bucket` is a name you cannot create a bucket with, even if a bucket with that name does not exist in your account. `trial-bucket` has been used by some one, some where. Go ahead, give it a try. 🤯

This is also why, if you are trying to create a s3 bucket, you may see this error message (or a version of it):
`An error occurred (IllegalLocationConstraintException) when calling the CreateBucket operation: The unspecified location constraint is incompatible for the region specific endpoint this request was sent to.`

## XHTML syntax for inserting code blocks in XHTML based wikis as an example

First, insert `DOCTYPE` before the `<head>` tag of the page if it is not existing. This will ensure that the browser uses a strict formatting, and is backward compatible. Without the `DOCTYPE` browsers use [quirks](https://developer.mozilla.org/en-US/docs/Web/HTML/Quirks_Mode_and_Standards_Mode) mode (before web standards). I have used "strict" DTD. You can choose to use any one of the 3 available [DTDs](https://www.tutorialspoint.com/xhtml/xhtml_doctypes.htm)

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

<head>
  ....
</head>
```

Next, wrap your text that you want to represent within code block with `<pre>... Your text ... </pre>` tags. That simple. Took me a whole day trying with `<code>`, `<samp>`, `<kbd>`

<pre>
  code block
  code block
</pre>

## CSS Validation service

[Go here](http://jigsaw.w3.org/css-validator/)

## XHTML Validation service

[Go here](https://validator.w3.org/)

## Cross account access for Glue to S3

Prefer to use IAM role among the 2 available [AWS Glue methods to grant cross account access](https://docs.aws.amazon.com/glue/latest/dg/cross-account-access.html). This is because IAM roles will work across cross-partition accounts (i.e., cross account access between aws and aws-cn) whereas resource based policies cannot cross partitions. 

Situation:
* Account `123456789012` contains a s3 bucket (say, `arn:aws:s3:::source-data-bucket`) from which data needs to be pulled
* Account `456789012345` contains a Glue crawler and catalog resources which will crawl and gather data

High level steps: 
* Definitely remove the ACL from the bucket in account `123456789012`
* Before setting up the Glue crawler, first prepare the cross account access role for Glue crawler in account `456789012345`. Do that by first creating an IAM policy in account `456789012345` as below: 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::source-data-bucket/<prefix>/*"
            ]
        }
    ]
}
```
`<prefix>` is the s3 bucket prefix, if you have specified any. I have used this to pull cost & usage data from CUR, hence a prefix was set in the CUR report definition.

Then create a IAM role (e.g., `CrossAccountGlueAccessForS3`) which should include the policy created above plus `AWSGlueServiceRole` (arn:aws:iam::aws:policy/service-role/AWSGlueServiceRole) managed policy. Add Glue in the Trust Relationship:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "glue.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

* Then set up your Glue crawler in account `456789012345` using this new role (`CrossAccountGlueAccessForS3`) that you have created. If unfamiliar, you can set up Glue crawler following the instructions [here](https://docs.aws.amazon.com/glue/latest/dg/console-crawlers.html)
* Go back to account `123456789012`. Create a bucket policy including the below statement for the bucket (`arn:aws:s3:::source-data-bucket`) in question.

```
{
    "Sid": "CrossAccountGlueAccess",
    "Effect": "Allow",
    "Principal": {
        "AWS": [
            "arn:aws:iam::456789012345:role/service-role/CrossAccountGlueAccessForS3"
        ]
    },
    "Action": [
        "s3:GetObject",
        "s3:ListBucket"
    ],
    "Resource": [
        "arn:aws:s3:::source-data-bucket",
        "arn:aws:s3:::source-data-bucket/*"
    ]
}
```

I needed to only scope down the policy to using two s3 permissions - `GetObject` and `ListBucket`. It is upto you to specify what actions you will allow the role from account `456789012345` to perform.

## To VSCode from PyCharm

Just to quick note for self and those wandering souls - if you change a code file in VSCode you need to `save` it first before it can take effect - like in your `git status`, `cdk synth`, or `cdk diff` (for AWS CDK) calls. In PyCharm, you don't need to. 
