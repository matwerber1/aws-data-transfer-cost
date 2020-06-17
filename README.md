# aws-data-transfer-cost

This document summarizes my understanding of data transfer charges as of June 2020. Note this is unofficial, please double-check AWS documentation for latest pricing.

## EC2 to S3

**Data transfer in to Amazon S3** [is free under normal circumstances](https://aws.amazon.com/s3/pricing/) *unless* you are using [AWS S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html). If you are using transfer acceleration, there is an S3 data transfer charge of either [$0.04 or $0.08 / GB of accelerated transfer](https://aws.amazon.com/s3/pricing/) (you will only be charged if we determine the acceleration is likely to improve your tranfer rate based on the location of the source data).

While the S3 service itself does not charge for inbound data (subject to the transfer acceleration caveat above), the *originating* services (e.g. EC2, VPC, etc.) might have **outbound** data transfer charges. These are discussed below:

[**Data transfer out from EC2** to an S3 bucket in the same region is free](https://aws.amazon.com/s3/pricing/). If an EC2 transfers data out to an S3 bucket in a different region, [it is charged at either $0.01 for the Ohio region and $0.02 / GB for all other AWS regions](https://aws.amazon.com/ec2/pricing/on-demand/).

**If your EC2 instance is in a private subnet and uses an AWS NAT Gateway (NGW)** to send data to Amazon S3, the NAT Gateway also charges *approximately* $0.045 / GB of data that passes through the NGW. [Prices vary slightly, depending on the region of the NGW](https://aws.amazon.com/vpc/pricing/).

