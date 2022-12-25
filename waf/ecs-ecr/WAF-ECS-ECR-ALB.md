#How to set up ECS + ECR + ALB + WAF and test via k6

!NOTE: for this you need docker to be installed on your machine!

First we can create ECR repository
![img.png](assets/repository_creation.png)

Then we can see push commands to build and push our image:
![img_1.png](assets/push_commands.png)

Once its done, we may see our image in ECR:
![img_2.png](assets/ecr_image.png)

Now lets navigate to ECS and create cluster

!NOTE: here we will use default vpc!

![img_3.png](assets/cluster_creation_start.png)

Next we should create a task definition:
![img.png](assets/task_definition_creation_name.png)
![img_17.png](assets/task_definition_creation.png)
As image URI use tha URI from ECR with tag (latest is for the latest version)

Next configure env for fargate:
![img_5.png](assets/fargate_config.png)

Rest of configurations may be left as they are
![img_6.png](assets/task_definition_created.png)
!NOTE: here we have a second revision, but you will have the first one!


Now lets work with our security groups
This one represents security group for our loadbalancer
![img_19.png](assets/alb_security_gr.png)
And this one accepts traffic from alb and passes to ECS on 8080
![img_20.png](assets/service_security_group.png)

Next navigate to cluster and create service to run our fargate:
![img_10.png](assets/serivce_creation_env.png)
![img_12.png](assets/service_creation_deployment.png)

In network choose our security group:
![img_22.png](assets/service_creation_network.png)

And create Load balancer right here:
![img_16.png](assets/service_creation_alb.png)
And after a few minutes we can see our service and our task in Running status in cluster console:
![img_14.png](assets/service_running.png)
![img_15.png](assets/task_running.png)



In alb console add our security group:
![img_21.png](assets/alb_sg_added.png)

Than pass our loadbalancer dns in url with our request uri and thats it:
![img_23.png](assets/alb_works.png)

Now lets create WAF as before and name it ecs-alb-waf:
![img_24.png](assets/waf_name.png)

But this time lets block everything that not ends with "test"
![img_25.png](assets/waf_rule.png)

And now we have HTTP 403 which is default WAF code:
![img_26.png](assets/waf_works.png)

Even in case that we dont have such endpoint:
![img_27.png](assets/waf_works_2.png)
![img_28.png](assets/waf_works_3.png)

Now lets test it via [k6](../../k6/K6.md):

This is with the correct url that not blocked with WAF:
![img_29.png](assets/k6_good_scenario.png)

Now lets create wrong URL:
![img_30.png](assets/k6_url.png)

And thats will be our output:
![img_31.png](assets/k6_bad_scenario.png)

As well as our log insights:
![img_33.png](assets/log_insights.png)