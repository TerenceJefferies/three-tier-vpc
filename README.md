# Three Tier VPC

This demonstration project is designed to show how one of the most common web architectures can be configured on AWS
using Terraform. The architecture consists of three tiers:

### 1. Presentation Tier

This tier gives users the ability to access the application via the internet. In configurations typical configurations, 
this layer would contain one or more load balancers, forwarding requests to the application tier.

### 2. Application Tier

In the application tier, the application compute resources are provisioned and run. By provisioning these resources in
a private subnet, we can avoid unwanted access to the application from the internet, giving us a reasonable layer of
security. 

### 3. Data Tier

As with the application tier, this tier is provisioned in a private subnet, hosting one or more databases.
By hosting our databases in a private subnet, we get an additional layer of network security, as with our application 
resources. In addition to this, extra security can be put in-place to ensure that only the application tier can access 
our database. This can be achieved using security groups.

## Provisioning

### Input Variables

All variables in this project are optional, but the following variables are available:

| Variable     | Description                               | Default          |
|--------------|-------------------------------------------|------------------|
| `aws_region` | The AWS region to provision the server in | `us-east-1`      |
| `vpc_name`   | The name of the new VPC                   | `three-tier-vpc` |

### Output Variables

The following output is available:

| Variable     | Description                               |
|--------------|-------------------------------------------|
| `vpc_id`     | The ID of the new VPC                     |
| `presentation_subnet_id` | The ID of the new presentation subnet |
| `application_subnet_id` | The ID of the new application subnet |
| `data_subnet_id` | The ID of the new data subnet |

### Executing

To provision these resources, run `terraform apply`

### Removing

To remove these resources, run `terraform destroy`

## Other Considerations

### Accessing the internet from private subnets

Resources hosted in private subnets are not capable of accessing the internet, this can be a problem for resources that
need to talk to 3rd party APIs or to download software updates. To overcome this, we can use a NAT Gateway. The 
NAT Gateway would need to be provisioned in a public subnet with a route to an internet gateway to achieve this.

### Accessing internal AWS resources from private subnets

It can be costly to access services such as S3 or DynamoDB from within a private subnet. Data transfer charges will
apply as your applications communicate with the AWS public zone. To overcome this, we can use a VPC Endpoint. An 
endpoint provides a network interface or gateway to allow you to "shortcut" access to the AWS public zone by routing
traffic via the endpoint/gateway instead of via the internet. This can be significantly cheaper than going via the 
internet if your application is making a lot of requests to AWS services. The cost saving varies depending on the
type of gateway/interface you require.
