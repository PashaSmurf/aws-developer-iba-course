# ECS, ECR
## Deploy web application using ECS Based on Ec2 & Fargate
### 1. Deploy ECS task using EC2
1. Create EC2 cluster (with new VPC and subnets)
![](assets/ecs_1_new_cluster.png)
2. Create a new Task Definition with Java container (0-8080 port) and default networking
![](assets/ecs_1_new_td.png)
3. Create a new Task in Cluster:
![](assets/ecs_1_new_task.png)

### 2. Deploy ECS Task using Fargate
1. Create VPC (using CloudFormation from previous practice);
2. Create ECR private repo;
![](assets/ecs_2_private_ecr.png)
3. After, let's login to ECR from our local Terminal (just copy command from ECR table):
```shell
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 250958575382.dkr.ecr.us-east-1.amazonaws.com
docker pull gazgeek/springboot-helloworld
docker tag gazgeek/springboot-helloworld:latest 250958575382.dkr.ecr.us-east-1.amazonaws.com/ecs-2:latest
docker push 250958575382.dkr.ecr.us-east-1.amazonaws.com/ecs-2:latest
```
![](assets/ecs_2_new_java_image.png)
4. Next create ALB with target group and new SG (all with our custom VPC):
![](assets/ecs_2_new_alb.png)
5. Next create Networking only cluster
![](assets/ecs_2_new_cluster.png)
6. Go to Task Definitions and create a new one - Fargate, with our VPC, 2 private subnets, Java image (with new SG) and ALB/TG
![](assets/ecs_2_new_td.png)
7. Go to Cluster -> Services -> create a new one (we should run our new TD)
8. After that we will have new tasks in Cluster tasks
![](assets/ecs_2_check_two_tasks.png)
![](assets/ecs_2_check_cluster.png)
9. Check it using ALB link
![](assets/ecs_2_test_java_alb.png)
10. Next create a new record in Route53 (alias and ALB) and test it
![](assets/ecs_2_check_dns.png)
11. Remove all stuff

### 3. Create the same setup as above for ECS using Cloudformation
1. Run VPC cloudformation `ecs_3_vpc.yaml`;
2. Run ECR cloudformation `ecs_3_ecr.yaml`;
3. Push Docker Java image to ECR;
4. Run ECS cloudformation `ecs_3_ecs.yaml`
![](assets/ecs_3_result.png)
![](assets/ecs_3_metric.png)