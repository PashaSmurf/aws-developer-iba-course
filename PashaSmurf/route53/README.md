# Route53
## 1. Assign A record for your EC2
1. Create a VPC using CloudFormation yaml file (with at tleast one public and private subnets).
2. Create an EC2 instance with httpd and default html page in Public subnet. Test it using Public IP:
![](assets/route_1_new_ec2.png)
![](assets/route_1_public_ip_check.png)
3. Let's go to Route53 -> Hosted Zones -> and create a new record with record type `A`, policy `Simple routing`,
our Public IP address and name `pavel`:
![](assets/route_1_create_a_record.png)
4. Check it using link `pavel.awscert.link` - WORKS!
![](assets/route_1_check_a_record.png)
5. Remove `A` record.

## 2. Alias setup for EC2
1. Let's use an EC2 instance from previous task
2. Create an ALB (with our VPC public subnets, Internet-Facing) and target group with our EC2 instance
![](assets/route_2_new_alb.png)
3. Check this ALB using DNS link from ALB settings:
![](assets/route_2_alb_public_check.png)
4. Let's create alias record using our region and ALB:
![](assets/route_2_create_alias_record.png)
5. Check it
![](assets/route_2_check_alias.png)
6. Delete ALB, EC2 instance and alias record.

## 3. Private hosted zone
1. Let's create an EC2 instance in Private subnet:
![](assets/route_3_new_private_instance.png)
2. Create Internal ALB and target group:
![](assets/route_3_new_internal_alb.png)
3. Create an alias record with this ALB - Timeout Error:
![](assets/route_3_new_alias_record.png)
4. Create a Bastion Host and try to ping it from Hosted Zone (using HealthCheck f.e.). But first of all add All ICMPv4 rule to BastionHost SG:
![](assets/route_3_bastion_host.png)
5. Create a HealthCheck for public IP - Works
![](assets/route_3_hc_bastion_host.png)
6. Remove all instances, records and VPC using CloudFormation

Questions
CNAME vs alias:
CNAME only for non-root domain (f.e. something.mydomain.com), points hostname to another hostname, we can point to EC2
Alias - points hostname to AWS resource, works for non-root AND root domain (mydomain.com), FREE.

Simple routing lets you configure standard DNS records, with no special Route 53 routing such as weighted or latency. 
With simple routing, you typically route traffic to a single resource, for example, to a web server for your website.

Failover routing lets you route traffic to a resource when the resource is healthy or to a different resource 
when the first resource is unhealthy.

Geolocation routing lets you choose the resources that serve your traffic based on the geographic location of your 
users, meaning the location that DNS queries originate from.

Geoproximity routing lets Amazon Route 53 route traffic to your resources based on the geographic location 
of your users and your resources.

If your application is hosted in multiple AWS Regions, you can improve performance for your users by serving 
their requests from the AWS Region that provides the lowest latency.

Weighted routing lets you associate multiple resources with a single domain name (example.com) or
subdomain name (acme.example.com) and choose how much traffic is routed to each resource. 