+++
title = "Infrastructure as Code"
menuTitle = "IaC"
weight = 10
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Infrastructure

Infrastructure refers to the underlying **components** and **resources** required for the operation of systems, applications, or services.

It is all the behind-the-scenes stuff that allows us to use computers, access websites, and use apps on our phones.

Components and resources are below

> #### Physical Components

Servers, computers, network devicdes (routers, switches), storage devices (hard drives, solid-state drives), and cables.

> #### Networking

IP addresses, subnets, routing, firewalls, and load balancers. Networking infrastructure enables communication between devices and facilitates the transfer of data.

> #### Operating Systems

Software that manages computer hardware. Windows, Linux, or macOS.

> #### Virtualization and Clouding Computing

Virtual instances of servers, storage, and etc. Platforms such as AWS, Azure or Google Cloud.

> #### Data Storage and Databases

File systems, storage area networks (SAN), object storage, and different database types.

> #### Security and Monitoring

Authentication, authorization, encryption, and network security measures.

> #### IaC

IaC is an approach where infrastructure is defined and managed through code. like AWS CloudFormation, Terraform

- Terraform : for ec2, not serverless
- Cloud Formation

SAM : Serverlss Application Model

## IaC

Infrastructure as Code (IaC) is the managing and provisioning of infrastructure through code instead of through manual processes. **AWS CloudFormation** is one of example of IaC.

![sst](https://d33wubrfki0l68.cloudfront.net/8d8130ee0ddaa46f5d0b19dffe1de91fff238aeb/23c47/assets/diagrams/how-cloudformation-works.png)

#### AWS CloudFormation

In stack, create Cognito, S3, DB, IAM..

#### AWS CDK

AWS CDK (Cloud Development Kit) allows you to use TypeScript, JavaScript, Java, .NET, and Python to create AWS infrasturcture.

SST comes with a list of higher-level CDK constructs.

It is a framework that makes it easy to build full-stack applications on AWS
It will automatically connect to the AWS with your $ aws configure information (created on AWS CLI day)

## AWS CLI

AWS CLI is a command line tool that allows you to interact with AWS services from the command line.

#### Install

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

- It is a framework that makes it easy to build full-stack applications on AWS
- It will automatically connect to the AWS with your `aws configure` information (created on AWS CLI day)

## Node.js AWS CLI

https://www.sammeechward.com/node-lambda-cli

## Dotnet AWS CLI

https://www.sammeechward.com/csharp-dotnet-aws-lambda

```c#
using Amazon.Lambda.Core;
using System.Net;
using Amazon.Lambda.APIGatewayEvents;

// Assembly attribute to enable the Lambda function's JSON input to be converted into a .NET class.
[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.DefaultLambdaJsonSerializer))]

namespace MyFunctionName;
public class Function
{
  public APIGatewayHttpApiV2ProxyResponse FunctionHandler(APIGatewayHttpApiV2ProxyRequest request, ILambdaContext context)
  {

    var response = new APIGatewayHttpApiV2ProxyResponse
    {
      StatusCode = (int)HttpStatusCode.OK,
      // json body
      Body = System.Text.Json.JsonSerializer.Serialize(new { Message = "Hello World" }),
      Headers = new Dictionary<string, string> { { "Content-Type", "application/json" } }
    };

    return response;
  }
}
```

Add the `Amazon.Lambda.APIGatewayEvents` package to your C# project:

```
dotnet add package Amazon.Lambda.APIGatewayEvents
```

And add the function to an API gateway endpoint.

You can query details about a function using this command:

```
aws lambda get-function --function-name my-lambda-function
```

#### All Cloud

- Terraform
- CloudFormation (AWS)

#### Serverless

- SAM (AWS)
- Serverless Framework
