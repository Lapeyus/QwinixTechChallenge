<h1>Challenge - Infrastructure as Code</h1>

<h2>Deliverable:</h2>
A single cloud formation template containing the described features stored in a github repository.  

<h2>Description:</h2>
<p>This challenge requires you to write a Cloudformation template that can be used to provision AWS services using automation. If you are not familiar with Cloudformation, you can search the Internet where there is a lot of information available from AWS about it. The CloudFormation template should be made available for review on github with full history of changes and commits using the master branch.</p>


This section gives a text description and creates the list of the instance types to the initiator of the stack, this way is able to specify the type of instance when the template is run from the given list.
 
  
 <code>
 Description": "Qwinix Tech Challenge",<br>
  <br>"Parameters": {
    <br>"InstanceType": {
      <br>"Description": "Choose your instance type",
      <br>"Type": "String",
      <br>"Default": "t1.micro",
      <br>"AllowedValues": ["t1.micro", "t2.micro", "m1.small", "m1.medium", "m1.large", "m3.medium", "m3.large", "m3.xlarge", "m3.2xlarge", "c1.medium", "r3.large"],
      <br>"ConstraintDescription": "Must be a valid EC2 instance type"
    <br>}
  <br>}
 </code>
