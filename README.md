<h1>Challenge - Infrastructure as Code</h1>

<h2>Deliverable:</h2>
A single cloud formation template containing the described features stored in a github repository.  

<h2>Description:</h2>
<p>This challenge requires you to write a Cloudformation template that can be used to provision AWS services using automation. If you are not familiar with Cloudformation, you can search the Internet where there is a lot of information available from AWS about it. The CloudFormation template should be made available for review on github with full history of changes and commits using the master branch.</p>

<code>
 "Parameters": {
    "InstanceType": {
      "Description": "EC2 instance type",
      "Type": "String",
      "Default": "t1.micro",
      "AllowedValues": [        "t1.micro",        "t2.micro",        "m1.small",        "m1.medium",        "m1.large",
        "m3.medium",        "m3.large",        "m3.xlarge",        "m3.2xlarge",        "c1.medium",        "r3.large"],
      "ConstraintDescription": "must be a valid EC2 instance type."
    }
  },
  </code>
<h3>The stack should initiate 3 EC2 instances accomplished using AutoScalingGroup</h3>
<code>
"WebServerGroup": {
      "Type": "AWS::AutoScaling::AutoScalingGroup",
      "Properties": {
        "AvailabilityZones": {
          "Fn::GetAZs": ""
        },
        "LaunchConfigurationName": {
          "Ref": "LaunchConfig"
        },
        "MinSize": "3",
        "MaxSize": "4",
        "DesiredCapacity": "3",
        "LoadBalancerNames": [{
          "Ref": "ElasticLoadBalancer"
        }]
      },
      "CreationPolicy": {
        "ResourceSignal": {
          "Timeout": "PT5M",
          "Count": "3"
        }
      },
<h3>The stack should maintain 3 instances running at any given time</h3>. 
      "UpdatePolicy": {
        "AutoScalingRollingUpdate": {
          "MinInstancesInService": "3",
          "MaxBatchSize": "1",
          "PauseTime": "PT15M",
          "WaitOnResourceSignals": "true"
        }
      },
      }
    },
  </code>

<h3>The ec2 instances should be configured with a security group that only allows the website traffic and ssh	</h3>
<h3>The ec2 instances should be configured with apache listening on port 8080.</h3> 
<h3>The webserver must display “Hello World” when the website is accessed on the ec2 instance</h3>
<h3>The only way to reach the apache on the instances is via an ELB that the stack also creates sending traffic from a public subnet, listening on port 80.</h3>
