# terraform-aws-vpc

A robust, reusable Terraform module for provisioning and managing AWS Virtual Private Clouds (VPCs) with best practices. This repository encapsulates all the necessary Terraform configuration to deploy a highly available, secure, and customizable VPC environment on AWS, making it easy to integrate into your cloud infrastructure workflows.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Usage](#usage)
- [Inputs](#inputs)
- [Outputs](#outputs)
- [File Structure](#file-structure)
  - [main.tf](#maintf)
  - [variables.tf](#variablestf)
  - [outputs.tf](#outputstf)
  - [data.tf](#datatf)
  - [locals.tf](#localstf)
- [Why Use data.tf and locals.tf?](#why-use-datatf-and-localstf)
- [Requirements](#requirements)
- [Authors](#authors)
- [License](#license)

---

## Overview

This Terraform module simplifies the process of creating a production-ready AWS VPC. It supports multiple subnets, route tables, NAT gateways, network ACLs, and more, while making it easy to customize networking according to your project needs.

## Features

- Customizable VPC CIDR block
- Public, private, and database subnets
- Multi-AZ deployment for high availability
- Support for NAT Gateways and Internet Gateways
- Route Table and Network ACL configuration
- Security Groups management
- Tagging for all resources
- Supports Terraform state management best practices

## Usage

```hcl
module "vpc" {
  source = "github.com/BharathKumarReddy2103/terraform-aws-vpc"

  vpc_name             = "my-vpc"
  vpc_cidr_block       = "10.0.0.0/16"
  public_subnets       = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnets      = ["10.0.101.0/24", "10.0.102.0/24"]
  enable_nat_gateway   = true
  azs                  = ["us-west-2a", "us-west-2b"]
  ...
}
```

## Inputs

| Name                  | Description                           | Type     | Default            | Required |
|-----------------------|---------------------------------------|----------|--------------------|----------|
| vpc_name              | Name for the VPC                      | string   | `null`             | yes      |
| vpc_cidr_block        | CIDR block for the VPC                | string   | `null`             | yes      |
| public_subnets        | List of public subnet CIDRs           | list     | `[]`               | yes      |
| private_subnets       | List of private subnet CIDRs          | list     | `[]`               | yes      |
| azs                   | List of availability zones            | list     | `[]`               | yes      |
| enable_nat_gateway    | Create NAT Gateway(s)                 | bool     | `true`             | no       |
| ...                   | ...                                   | ...      | ...                | ...      |

> **Note:** For a complete list of variables, review the [variables.tf](variables.tf) file.

## Outputs

| Name              | Description                                |
|-------------------|--------------------------------------------|
| vpc_id            | The ID of the VPC                          |
| public_subnet_ids | List of public subnet IDs                  |
| private_subnet_ids| List of private subnet IDs                 |
| ...               | ...                                        |

> **Note:** For a complete list of outputs, review the [outputs.tf](outputs.tf) file.

## File Structure

```
terraform-aws-vpc/
├── main.tf
├── variables.tf
├── outputs.tf
├── data.tf
├── locals.tf
├── README.md
```

### main.tf

Contains the main Terraform resources for creating the VPC, subnets, gateways, route tables, security groups, etc.

### variables.tf

Defines all input variables required or optional for this module, enabling customization and reuse.

### outputs.tf

Specifies the output values, such as VPC ID, subnet IDs, and NAT Gateway IDs, that are useful for integrations with other modules or root Terraform configurations.

### data.tf

`data.tf` is used to define **data sources**—read-only information that is fetched from AWS or other Terraform providers. For example, it can be used to fetch information about available AWS Availability Zones, existing VPCs, AMIs, etc., which can then be referenced in your resource definitions.

**Why use `data.tf`?**
- Ensures your configuration adapts to dynamic AWS environment changes (e.g., always using the latest available AZs).
- Allows referencing existing resources without managing them via Terraform.
- Reduces hardcoding and increases portability.

### locals.tf

`locals.tf` is used to define **local values**—named values that can be computed from input variables, data sources, or other locals. These help to keep the code DRY (Don't Repeat Yourself), improve readability, and encapsulate complex expressions.

**Why use `locals.tf`?**
- Simplifies complex expressions by breaking them into manageable named values.
- Centralizes repeated values or logic.
- Makes the configuration easier to maintain and update.

## Why Use data.tf and locals.tf?

- **data.tf**: Fetches existing AWS information dynamically, such as AZs or VPC attributes, so your infrastructure remains up-to-date and adaptable.
- **locals.tf**: Defines computed values or reusable expressions to avoid repetition and simplify complex logic, making your codebase cleaner and more maintainable.

Example:

```hcl
# data.tf
data "aws_availability_zones" "available" {}

# locals.tf
locals {
  azs = slice(data.aws_availability_zones.available.names, 0, 2)
}
```

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) >= 1.0.0
- AWS Account with appropriate IAM permissions

## Authors

- [Bharath Kumar Reddy](https://github.com/BharathKumarReddy2103)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
