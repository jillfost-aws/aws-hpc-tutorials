---
title: "Simulations with AWS Batch"
date: 2021-10-08T09:05:54Z
weight: 60
pre: "<b>Lab IV ⁃ </b>"
tags: ["HPC", "Overview", "Batch"]
---

<!--
**Fully managed batch processing at any scale**

AWS Batch enables developers, scientists, and engineers to easily and efficiently run hundreds of thousands of batch computing jobs on AWS. AWS Batch dynamically provisions the optimal quantity and type of compute resources (e.g., CPU or memory optimized instances) based on the volume and specific resource requirements of the batch jobs submitted. With AWS Batch, there is no need to install and manage batch computing software or server clusters that you use to run your jobs, allowing you to focus on analyzing results and solving problems. AWS Batch plans, schedules, and executes your batch computing workloads across the full range of AWS compute services and features, such as Amazon EC2 and Spot Instances.

There is no additional charge for AWS Batch. You only pay for the AWS resources (e.g. EC2 instances) you create to store and run your batch jobs.
For more details on that topic see the [AWS Batch](https://aws.amazon.com/batch/)

Key advantages of AWS Batch

AWS Batch eliminates the need to operate third-party commercial or open source batch processing solutions. There is no batch software or servers to install or manage. AWS Batch manages all the infrastructure for you, avoiding the complexities of provisioning, managing, monitoring, and scaling your batch computing jobs.

**Fully managed**

AWS Batch eliminates the need to operate third-party commercial or open source batch processing solutions. There is no batch software or servers to install or manage. AWS Batch manages all the infrastructure for you, avoiding the complexities of provisioning, managing, monitoring, and scaling your batch computing jobs.

**Integrated with AWS**

AWS Batch is natively integrated with the AWS platform, allowing you to leverage the scaling, networking, and access management capabilities of AWS. This makes it easy to run jobs that safely and securely retrieve and write data to and from AWS data stores such as Amazon S3 or Amazon DynamoDB.

**Cost optimized resource provisioning**
AWS Batch provisions compute resources and optimizes the job distribution based on the volume and resource requirements of the submitted batch jobs. AWS Batch dynamically scales compute resources to any quantity required to run your batch jobs, freeing you from the constraints of fixed-capacity clusters. AWS Batch will utilize Spot Instances on your behalf, reducing the cost of running your batch jobs further. -->

{{% notice info %}}
This lab requires an AWS Cloud9 IDE. If you do not have an AWS Cloud9 IDE set up, complete the **[Prepartion](/02-aws-getting-started.html)** section of the workshop
{{% /notice %}}

In this lab, you will learn how to use container orchestrators such as AWS Batch and deploy an architecture for genomics pipeline execution on AWS Batch.

You will run a basic pipeline for quantification of genomic features from short read data implemented with [Nextflow](https://www.nextflow.io/index.html)

You will be deploying the below architecture as part of this lab:

![AWS BATCH](/images/aws-batch/SCA23/lab4-intro-arch.png)

This includes the following steps:

- [Create S3 bucket](06-batch-automation/01-create-s3.html)
- [Create IAM Role](06-batch-automation/02-create-iam-role.html)
- [Set up Batch Resources](06-batch-automation/03-setup-batch-ce-jq.html)
- [Set up a Job Definition](06-batch-automation/04-setup-batch-job-definition.html)
- [Describe Your Environment](06-batch-automation/05-describe-batch-env.html)
- [Run a Single Job](06-batch-automation/06-run-job.html)
- [Monitor your jobs](06-batch-automation/07-monitor-job.html)
- [Cleanup](06-batch-automation/08-cleanup.html)

