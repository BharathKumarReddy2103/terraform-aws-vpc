# Terraform AWS VPC Module

A reusable, customizable, and production-ready Terraform module for creating and managing AWS Virtual Private Clouds (VPCs). This module simplifies the process of provisioning VPCs and related networking resources, enabling you to deploy secure and scalable network infrastructure in AWS with ease.

## Features

- Create a new VPC with configurable CIDR blocks
- Support for public, private, and database subnets (multi-AZ)
- Automated creation of route tables, internet gateways, and NAT gateways
- Optional support for VPC endpoints, flow logs, DHCP options, and more
- Tagging support for all resources
- Highly customizable via input variables
- Outputs for key identifiers and networking details

## Usage

```hcl
module "vpc" {
  source  = "github.com/BharathKumarReddy2103/terraform-aws-vpc"

  vpc_name             = "my-vpc"
  vpc_cidr_block       = "10.0.0.0/16"
  azs                  = ["us-east-1a", "us-east-1b", "us-east-1c"]

  public_subnets       = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  private_subnets      = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  enable_nat_gateway   = true
  single_nat_gateway   = true

  tags = {
    Environment = "production"
    Project     = "my-project"
  }
}
```

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 1.0 or later
- AWS provider

## Input Variables

| Name                 | Description                                   | Type     | Default           | Required |
|----------------------|-----------------------------------------------|----------|-------------------|:--------:|
| vpc_name             | Name for the VPC                              | string   | n/a               |   yes    |
| vpc_cidr_block       | CIDR block for the VPC                        | string   | n/a               |   yes    |
| azs                  | List of AZs to use                            | list     | n/a               |   yes    |
| public_subnets       | List of public subnet CIDRs                   | list     | n/a               |   yes    |
| private_subnets      | List of private subnet CIDRs                  | list     | n/a               |   yes    |
| enable_nat_gateway   | Enable/disable NAT Gateway                    | bool     | false             |   no     |
| single_nat_gateway   | Use single NAT Gateway for all AZs            | bool     | true              |   no     |
| tags                 | A map of tags to add to all resources         | map      | {}                |   no     |

> _See_ [variables.tf](./variables.tf) _for the full list of available variables and customization options._

## Outputs

- VPC ID
- Subnet IDs (public and private)
- Route table IDs
- NAT Gateway IDs
- Additional resource outputs for integration

> _Refer to_ [outputs.tf](./outputs.tf) _for a full list of outputs._

## Examples

See [examples/](./examples) directory for complete usage examples.

## Contributing

Contributions, issues, and feature requests are welcome!

1. Fork this repository
2. Create a new branch (`git checkout -b feature/my-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/my-feature`)
5. Open a pull request

## License

This repository is licensed under the [MIT License](./LICENSE).

---

**Author:** [Bharath Kumar Reddy](https://github.com/BharathKumarReddy2103)
