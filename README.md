# aws-data-transfer-cost

This document summarizes my understanding of data transfer charges as of June 2020. Note this is unofficial, please double-check AWS documentation for latest pricing.

## Requests

If you'd like me to research a use case and document it here, please open an issue on this project. 

## EC2 to S3

**Data transfer in to Amazon S3** [is free under normal circumstances](https://aws.amazon.com/s3/pricing/) *unless* you are using [AWS S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html). If you are using transfer acceleration, there is an S3 data transfer charge of either [$0.04 or $0.08 / GB of accelerated transfer](https://aws.amazon.com/s3/pricing/) (you will only be charged if we determine the acceleration is likely to improve your tranfer rate based on the location of the source data).

While the S3 service itself does not charge for inbound data (subject to the transfer acceleration caveat above), the *originating* services (e.g. EC2, VPC, etc.) might have **outbound** data transfer charges. These are discussed below:

[**Data transfer out from EC2** to an S3 bucket in the same region is free](https://aws.amazon.com/s3/pricing/). If an EC2 transfers data out to an S3 bucket in a different region, [it is charged at either $0.01 for the Ohio region and $0.02 / GB for all other AWS regions](https://aws.amazon.com/ec2/pricing/on-demand/).

**If your EC2 instance is in a private subnet and uses AWS NAT Gateway (NGW)** to send data to Amazon S3, the NAT Gateway also charges *approximately* $0.045 / GB of data that passes through the NGW. [Prices vary slightly, depending on the region of the NGW](https://aws.amazon.com/vpc/pricing/).

**If you have a private EC2 instance, you can eliminate the NGW outbound data transfer cost** by using an S3 VPC Gateway Endpoint (aka AWS PrivateLink for S3). PrivateLink creates a private, direct connnection between your VPC and the Amazon S3 service. [PrivateLink endpoints for S3 do not charge for data transfer to Amazon S3](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#gateway-endpoint-pricing). *Note* - PrivateLink endpoints are available for many AWS services; some of these endpoints *do* charge for data transfer, so be sure to check for each specific service. 

**If you have a public EC2 instance** that communicates with Amazon S3 over the public internet (via an Internet Gateway), then that EC2 instance must either have a public IP or a public Elastic IP address. [Data transferred on these public addresses is charged at $0.01 / GB](https://aws.amazon.com/ec2/pricing/on-demand/). Like the NAT Gateway charges, you can eliminate this charge by using an S3 PrivateLink Gateway / VPC Endpoint. 

