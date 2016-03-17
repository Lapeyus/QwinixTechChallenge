This section gives a text description and creates the list of the instance types to the initiator of the stack, this way is able to specify the type of instance when the template is run from the given list.


Description": "Qwinix Tech Challenge",
  "Parameters": {
    "InstanceType": {
      "Description": "Choose your instance type",
      "Type": "String",
      "Default": "t1.micro",
      "AllowedValues": ["t1.micro", "t2.micro", "m1.small", "m1.medium", "m1.large", "m3.medium", "m3.large", "m3.xlarge", "m3.2xlarge", "c1.medium", "r3.large"],
      "ConstraintDescription": "Must be a valid EC2 instance type"
    }
  }
