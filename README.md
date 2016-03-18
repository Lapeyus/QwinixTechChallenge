<h1>Challenge - Infrastructure as Code</h1>

<h2>Deliverable:</h2>
A single cloud formation template containing the described features stored in a github repository.  

<h2>Description:</h2>
<p>This challenge requires you to write a Cloudformation template that can be used to provision AWS services using automation. If you are not familiar with Cloudformation, you can search the Internet where there is a lot of information available from AWS about it. The CloudFormation template should be made available for review on github with full history of changes and commits using the master branch.</p>


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
The stack should initiate 3 EC2 instances accomplished using AutoScalingGroup
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
The stack should maintain 3 instances running at any given time. 
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

