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
<code>
"WebServerSecurityGroup": {
      "Type": "AWS::EC2::SecurityGroup",
      "Properties": {
        "GroupDescription": "Enable HTTP access via port 80 locked down to the ELB and SSH access",
        "SecurityGroupIngress": [{
          "IpProtocol": "tcp",
          "FromPort": "80",
          "ToPort": "80",
          "SourceSecurityGroupOwnerId": {
            "Fn::GetAtt": [
              "ElasticLoadBalancer",
              "SourceSecurityGroup.OwnerAlias"
            ]
          },
          "SourceSecurityGroupName": {
            "Fn::GetAtt": [
              "ElasticLoadBalancer",
              "SourceSecurityGroup.GroupName"
            ]
          }
        }, {
          "IpProtocol": "tcp",
          "FromPort": "22",
          "ToPort": "22",
          "CidrIp": "0.0.0.0/0"
        }]
      },
      "Metadata": {
        "AWS::CloudFormation::Designer": {
          "id": "914d06dc-c71a-4470-bb25-b9e6763dc6db"
        }
      }
    },
</code>
<h3>The ec2 instances should be configured with apache listening on port 8080.</h3> 
<code>
"LaunchConfig": {
      "Type": "AWS::AutoScaling::LaunchConfiguration",
      "Metadata": {
        "Comment1": "Configure the bootstrap helpers to install the Apache Web Server and PHP",
        "Comment2": "The website content is created",
        "AWS::CloudFormation::Init": {
          "config": {
            "packages": {
              "yum": {
                "httpd": [],
                "php": [],
                "php-mysql": []
              }
            },
                </code>
<h3>The webserver must display “Hello World” when the website is accessed on the ec2 instance</h3>
<code>
            "files": {
              "/var/www/html/index.php": {
                "content": {
                  "Fn::Join": [
                    "", [
                      "<html>\n",
                      "  <head>\n",
                      "    <title>Qwinix Tech Challenge</title>\n",
                      "    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=ISO-8859-1\">\n",
                      "  </head>\n",
                      "  <body>\n",
                      "    <h1>Hello world?</h1>\n",
                      "  </body>\n",
                      "</html>\n"
                    ]
                  ]
                },
</code>
<h3>The only way to reach the apache on the instances is via an ELB that the stack also creates sending traffic from a public subnet, listening on port 80.</h3>
<code>
    "ElasticLoadBalancer": {
      "Type": "AWS::ElasticLoadBalancing::LoadBalancer",
      "Properties": {
        "CrossZone": "true",
        "AvailabilityZones": {
          "Fn::GetAZs": ""
        },
        "LBCookieStickinessPolicy": [{
          "PolicyName": "CookieBasedPolicy",
          "CookieExpirationPeriod": "30"
        }],
        "Listeners": [{
          "LoadBalancerPort": "80",
          "InstancePort": "80",
          "Protocol": "HTTP",
          "PolicyNames": [
            "CookieBasedPolicy"
          ]
        }],
</code>

##Enable the autoscaling policy to scale up 1 server at a time based on a cpu alarm trigger of 80%
<code>
"CPUAlarmHigh": {
      "Type": "AWS::CloudWatch::Alarm",
      "Properties": {
        "AlarmDescription": "Scale-up if CPU > 80% for 10 minutes",
        "MetricName": "CPUUtilization",
        "Namespace": "AWS/EC2",
        "Statistic": "Average",
        "Period": "300",
        "EvaluationPeriods": "2",
        "Threshold": "80",
        "AlarmActions": [
          {
            "Ref": "WebServerScaleUpPolicy"
          }
        ],
        "Dimensions": [
          {
            "Name": "AutoScalingGroupName",
            "Value": {
              "Ref": "WebServerGroup"
            }
          }
        ],
        "ComparisonOperator": "GreaterThanThreshold"
      },
</code>
##Each ec2 instance must be tagged with name equaling yourname_test. For example tag key name would equal steve_test in my case. 
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
        }],
        "Tags": [{
          "Key": "MyTag",
          "Value": "joseph_test",
          "PropagateAtLaunch": "true"
        }]
</code>
