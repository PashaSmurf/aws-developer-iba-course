# EC2
## Task 1 - Launch internet accessible web server using ec2 with manual setup
1. Create a simple instance of EC2 without userdata;
![](assets/ec2_1_instances_list.png)
2. Open the console of this instance:
![](assets/ec2_1_instance_console.png)
3. Run some commands here:
```shell
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
sudo nano /var/www/html/index.html
```
In file paste this code:
```html
<h1>Hello World ES2-1</h1>
```
4. Check the result using public ip address:
![](assets/ec2_1_public_ip_check.png)
5. Terminate the instance.

## Task 2 - Setup EC2 using userdata
1. Create an instance of EC2 with preload userdata:
```shell
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo '<h1>Hello World EC2-2</h1>' | sudo tee /var/www/html/index.html
```
![](assets/ec2_2_instances_list.png)
2. Check the result using public ip:
![](assets/ec2_2_public_ip_check.png)
3. Terminate the instance.

## Task 3 - Setup EC2 using userdata and create a static public ip address.
1. Create an instance of EC2 with preload userdata:
```shell
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo '<h1>Hello World EC2-2</h1>' | sudo tee /var/www/html/index.html
```
![](assets/ec2_3_public_ip_check.png)
2. Create an Elastic IP:
![](assets/ec2_3_elastic_ip.png)
3. Associate Elastic IP with EC2 instance from previous step;
4. Check the Elastic IP in browser -> reboot instance -> check again THE SAME IP -> works!
5. Terminate the instance.

## Task 4 - Create an ec2 server and test access for S3.
1. Create a bucket with default settings in S3:
![](assets/ec2_4_bucket_list.png)
2. Try to check the list of available buckets in CloudShell:
```shell
aws s3 ls
```
![](assets/ec2_4_cloudshell.png)
3. Create the EC2 instance from 2 task.
4. Try check the same buckets from EC2 instance shell. Error
![](assets/ec2_4_instance_shell.png)
5. Create new Role in IAM-Roles with `AmazonS3ReadOnlyAccess` policy
![](assets/ec2_4_new_role.png)
6. Attach the new IBM role to a specific EC2 instance:
![](assets/ec2_4_attach_role.png)
7. Try to check again the list of S3 buckets from EC2 shell - works!:
![](assets/ec2_4_instance_shell_2.png)
8. Wait a bit before terminating. Will terminate after some steps in task 5.

## Task 5 - Launch template
1. Let's create template from EC2 `ec2-4` instance from previous task. After that we can terminate the instance:
![](assets/ec2_5_templates_list.png)
2. Next let's create an instance from template:
![](assets/ec2_5_instance_from_template.png)
3. Check the public ip of this instance:
![](assets/ec2_5_public_ip_check.png)
4. Terminate the instance.

## Task 6 - Setup highly available server stack using ALB + Target Group
1. Create new security group for EC2 instances - `EC2-target`
![](assets/ec2_6_new_security_group.png)
2. Create two new EC instances with new security group `EC2-target` and this userdata
```shell
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo '<h1>Hello World from First|Second instance</h1>' | sudo tee /var/www/html/index.html
```
![](assets/ec2_6_new_instances.png)
3. Create a target group with 2 EC2 instances:
![](assets/ec2_6_target_group_creation.png)
4. Create a new security group for ALB - `ALB-EC2-6`
![](assets/ec2_6_alb_security_group.png)
5. Create ALB
![](assets/ec2_6_load_balancer.png)
6. Check the DNS of balancer - works
![](assets/ec2_6_alb_first_instance.png)
![](assets/ec2_6_alb_second_instance.png)
7. To prevent connection directly to EC2 instances - let's update `EC2-target` security group
![](assets/ec2_6_update_security_group.png)
8. Now if we connect directly to EC2 - infinite loading
![](assets/ec2_6_infinite_loading.png)
9. Terminate ALB and all instances

## Task 7 - Spot Fleet
1. Create template using t2.micro, default userdata with httpd and index.html:
![](assets/ec2_7_launch_template.png)
2. Create a new target group:
![](assets/ec2_7_target_group.png)
3. Create a new ALB with the target group from previous step:
![](assets/ec2_7_alb.png)
4. Create ASG using template and ALB from previous steps:
![](assets/ec2_7_asg.png)
5. Check instance using ALB DNS:
![](assets/ec2_7_alb_check.png)
6. Create a spot request using template
![](assets/ec2_7_spot_request.png)
Instance:
![](assets/ec2_7_spot_instance.png)
7. Check using instance public ip
![](assets/ec2_7_public_check.png)
8. Remove all stuff!

## Task 8 - Create AMI with httpd server
1. Create a new EC2 instance with html page and httpd. Will use from template (from 5 task). Check the public ip:
![](assets/ec2_8_check_instance.png)
2. Create an AMI from this EC2 instance:
![](assets/ec2_8_images_list.png)
3. Remove the current EC2 instance and create a new one using AMI image:
![](assets/ec2_8_instance_from_ami.png)
4. Check the result using public ip:
![](assets/ec2_8_check_second_instance.png)
5. Remove AMI

## Task 9 - Launch Configuration and Autoscaling group
1. Create a new AMI from basic EC2 instance (httpd and html page):
![](assets/ec2_9_new_ami.png)
2. Create new target group:
![](assets/ec2_9_new_target_group.png)
3. Create new ALB with target group from previous step:
![](assets/ec2_9_new_alb.png)
4. Create new launch template using AMI from step 1:
![](assets/ec2_9_ami_launch_template.png)
5. Create an ASG using our template, balancer and target group. Set min - 1, des - 2, max - 3
![](assets/ec2_9_alg_result.png)
![](assets/ec2_9_instances.png)
![](assets/ec2_9_result_page.png)
6. Remove ASG, ALB, AMI and instances

## Task 10 - EBS and ec2
1. Create a new EC2 instance (f.e. from template):
![](assets/ec2_10_new_instance.png)
2. Create a new EBS volume in the same AZ as instance:
![](assets/ec2_10_new_volume.png)
3. Attach volume to our EC2 instance:
![](assets/ec2_10_attach_form.png)
The result is here:
![](assets/ec2_10_instance_volume_list.png)
4. We should make EBS available for use:
```shell
sudo mkfs -t xfs /dev/sdf #create a file system on the volume. ATTENTION!! If we know that the volume is empty
sudo mkdir /data
sudo mount /dev/sdf /data #mount the volume at the directory you created in the previous step
```
5. Add `test.txt` file to our volume:
```shell
echo 'Put some text to the file' | sudo tee text.txt
```
![](assets/ec2_10_new_instance_file.png)
6. After that we should detach EBS. First of all we should umount it from instance:
```shell
sudo umount -d /dev/sdf
```
7. Create a new instance with name `ec-2-10`
![](assets/ec2_10_second_instance.png)
8. Detach volume from first EC2 instance and attach to the second one:
![](assets/ec2_10_attach_form_2.png)
9. Connect to the second instance and mount the EBS:
```shell
sudo mkdir /data
sudo mount /dev/sdf /data
```
![](assets/ec2_10_check_second_instance.png)
10. Terminate all instances and extra volume.

## Task 11 - EFS and ec2
1. Create an EFS for several regions (because I want to check some EC2 instances with the same file storage).
A little remark. I created also new security group as in course. Attention. Don't forget to add new rule for SSH 22 port! 
![](assets/ec2_11_new_security_group.png)
![](assets/ec2_11_file_system.png)
2. Create two instances (one in A and one in B AZ) and attach the new file storage with the path `/mnt/efs/fs1`
![](assets/ec2_11_attach_file_system.png)
3. (a little remark. I didn't understand what does it mean - 'upload'. It's just a simple creating a file or we should
upload file to EC2 from our laptop storage)
I tried to upload using as usual `scp` but `ec2-user` doesn't have enough permissions. `root` user is disable by Amazon.
```shell
scp /Users/pavellahovskij/test_file.txt -i EC2-task.pem ec2-user@44.204.18.36:/mnt/efs/fs1/test_file.txt
```
So. Only simple creation
![](assets/ec2_11_file_creation.png)
4. This file also available from the second instance in another AZ
![](assets/ec2_11_another_instance.png)
5. Terminate instances and file storage.

## Task 12 - EBS and EC2
1. Create aa instance with default and custom volume with encryption, gp3 type and `/dev/sdy` name
![](assets/ec2_12_custom_volume.png)
2. Check that we have two volumes into one instance:
![](assets/ec2_12_check_volumes.png)
![](assets/ec2_12_check_volumes_2.png)
3. Terminate the EC2 instance and check volumes again - only custom volume is available
![](assets/ec2_12_check_volume_3.png)
4. Delete this custom volume

## Task 13 - Placement Group
1. Create all 3 types of placement groups:
![](assets/ec2_13_placement_groups.png)
2. Create 2 instances for each of placement group:
![](assets/ec2_13_placement_instances.png)
3. Check all public ips:
![](assets/ec2_13_cluster_check.png)
![](assets/ec2_13_spread_check.png)
![](assets/ec2_13_partition_check.png)
4. Terminate all instances and placement groups. A little summarize: I didn't see any changes 
but I checked the official docs and found that the differences between these placement groups are in locations in hardware - together 
or separately.
