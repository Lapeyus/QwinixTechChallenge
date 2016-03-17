Challenge - Infrastructure as Code

Deliverable:
A single cloud formation template containing the described features stored in a github repository.  

Description:
This challenge requires you to write a Cloudformation template that can be used to provision AWS services using automation. If you are not familiar with Cloudformation, you can search the Internet where there is a lot of information available from AWS about it. The CloudFormation template should be made available for review on github with full history of changes and commits using the master branch. 

    The stack should initiate 3 EC2 instances. 
    The stack should maintain 3 instances running at any given time. 
    The initiator of the stack should be able to specify the type of instance when the template is run from a list of the following:
                t1.micro
                t2.micro
                m1.small
                m1.medium
                m1.large
                m3.medium
                m3.large
                m3.xlarge
                m3.2xlarge
                c1.medium
                r3.large

    The instances should each be spawned in a different availability zone inside a private subnet. 
    The ec2 instances should be configured with a security group that only allows the website traffic and ssh
    The ec2 instances should be configured with apache listening on port 8080. 
    The webserver must display “Hello World” when the website is accessed on the ec2 instance
    Each ec2 instance must be tagged with name equaling yourname_test. For example tag key name would equal steve_test in my case. 
    The only way to reach the apache on the instances is via an ELB that the stack also creates sending traffic from a public subnet, listening on port 80.
    The stack should include an S3 bucket and instances should have a policy that allows putting and getting objects from that bucket.

This challenge also requires you to document your work in a separate file. The documentation should discuss all of the components in the template. You should explain briefly in a sentence or two each of the items you have included and what they do so that we can ensure you understand the template you have written. This document should also be included in the github repo for review. 

Bonus Features:
You can add these additional features to your Cloudformation template. Though they are not necessary to complete the challenge. Here are a few additional ideas.

    Enable the autoscaling policy to scale up 1 server at a time based on a cpu alarm trigger of 80%
    For each ec2 instance, automatically install a chef client on the server
