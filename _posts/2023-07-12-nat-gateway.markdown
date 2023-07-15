---
layout: post
title: "The cost of NAT Gateways"
date: 2023-07-12 21:18:00 -0700
categories: infrastructure
excerpt_separator: <!--ex-->
---

A Network Address Translation (NAT) Gateway is a service that is used to connect instances in a private VPC subnet to the public internet.

<!--ex-->

To allow your cloud instances to access the public internet, the instance usually either needs to have a public IP address or connects to a NAT gateway. However, NAT Gateway can be costly. This [article](https://medium.com/life-at-chime/how-we-reduced-our-aws-bill-by-seven-figures-5144206399cb) and similar [github issues](https://github.com/aws/aws-cdk/issues/18720) are examples of how NAT Gateway costs can quickly add up.

If we look at the current pricing for NAT Gateway on the major cloud platforms.

| Cloud Provider  | Cost per hour | Cost per GiB |
|-----------------|---------------|--------------|
| [AWS](https://aws.amazon.com/vpc/pricing/) (us-east-1) | $0.045        | $0.045       |
| [Azure](https://azure.microsoft.com/en-ca/pricing/details/virtual-network/#pricing)           | $0.045        | $0.045       |
| [Google Cloud](https://cloud.google.com/nat/pricing)    | $0.044        | $0.045       |

As a NAT Gateway is needed for each subnet. That's easily $32 per month per private subnet, not counting the cost of any egress data. For data intensive applications, the data egress would also be costly. That's why some companies have resolved to developing their own NAT instances.

AWS used to provide a [NAT instance AMI](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html) but the support is now deprecated. Digital ocean has some [documentation on setting up a NAT instance](https://docs.digitalocean.com/products/networking/vpc/how-to/configure-droplet-as-gateway/) which can be a much cheaper option. The user would only need to pay for the instance and the egress from the instance, and digital ocean droplets have [free egress allowance](https://docs.digitalocean.com/products/billing/bandwidth/#droplets). While setting up NAT instances can potentially reduce cost, it may impact the availability of the service. For more detailed comparison between self-hosted NAT instance and NAT gateway, see this [article](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-comparison.html). NAT Gateway also has other limitations. For example a single NAT Gateway on AWS can support only up to [100 Gbps of bandwidth and 10 million packets per second](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html#nat-gateway-basics).

# Glossary
Using AWS examples:
1. [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
2. [VPC Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html#subnet-basics)
3. [NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)