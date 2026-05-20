---
title: "Using Lambda-backed Custom Resources to Reduce Overhead in a Multi-Account Environment"
url: "https://aws.amazon.com/blogs/mt/using-lambda-backed-custom-resources-to-reduce-overhead-in-a-multi-account-environment/"
date: "Thu, 05 Oct 2023 15:21:59 +0000"
author: "Josh Rodgers"
feed_url: "https://aws.amazon.com/blogs/mt/category/management-tools/aws-control-tower/feed/"
---
Introduction Many of my customers use AWS CloudFormation to streamline provisioning operations for AWS and third-party resources, that they describe with code in JSON- or YAML-formatted CloudFormation templates. Some workloads require custom logic or inputs beyond standard parameter values. For these scenarios, an often overlooked and useful CloudFormation feature lies in AWS Lambda-backed custom resources.
