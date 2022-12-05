# Tasks with ec2-instance

### Launch internet accessible web server using ec2 with manual setup. Launch ec2 instance, setup web server using httpd and html page, test it via public ip address.

Connect to ec2-instance
![](assets/connect-to-ec2.png)

Write some commands:

- sudo su
- yum update -y
- yum install httpd -y
- service httpd start
- chkconfig httpd on

And create html file via "nano" command


Enter my Public IPv4 DNS from ec2-instance

![](assets/ec2-dns-name.png)

And you can see your result

![](assets/ended-task1.png)

Task ended!

### Setup EC2 using userdata. Automate step 1 using userdata. Do your steps to set up a web server via userdata, test it via a public ip address.

When we want to create ec2 instance with userdata, you need select "Advanced settings" 

Then you need enter your script such as :

/yum update -y

/yum install -y httpd

/systemctl start httpd

/systemctl enable httpd

/echo "(h1 Hello World /h1" > /var/www/html/index.html

#### When your instance is running copy your public IPv4 and past in browser

![](assets/public-ip.png)

#### You can see the result

![](assets/result-hello-world.png)

Task ended!



