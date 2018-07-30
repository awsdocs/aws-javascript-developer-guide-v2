# Tutorial: Setting Up Node\.js on an Amazon EC2 Instance<a name="setting-up-node-on-ec2-instance"></a>

A common scenario for using Node\.js with the SDK for JavaScript is to set up and run a Node\.js web application on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. In this tutorial, you will create a Linux instance, connect to it using SSH, and then install Node\.js to run on that instance\. 

## Prerequisites<a name="setting-up-node-on-ec2-instance.prerequisites"></a>

This tutorial assumes that you have already launched a Linux instance with a public DNS name that is reachable from the Internet and to which you are able to connect using SSH\. For more information, see [Step 1: Launch an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance_linux) in the *Amazon EC2 User Guide for Linux Instances*\.

You must also have configured your security group to allow `SSH` \(port 22\), `HTTP` \(port 80\), and `HTTPS` \(port 443\) connections\. For more information about these prerequisites, see [ Setting Up with Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Procedure<a name="setting-up-node-on-ec2-instance-procedure"></a>

The following procedure helps you install Node\.js on an Amazon Linux instance\. You can use this server to host a Node\.js web application\.

**To set up Node\.js on your Linux instance**

1. Connect to your Linux instance as `ec2-user` using SSH\.

1. Install node version manager \(nvm\) by typing the following at the command line\.

   ```
   curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
   ```

   We will use nvm to install Node\.js because nvm can install multiple versions of Node\.js and allow you to switch between them\. 

1. Activate nvm by typing the following at the command line\.

   ```
   . ~/.nvm/nvm.sh
   ```

1. Use nvm to install the version of Node\.js you intend to use by typing the following at the command line\.

   ```
   nvm install 8.11.3
   ```

   Installing Node\.js also installs the Node Package Manager \(npm\) so you can install additional modules as needed\.

1. Test that Node\.js is installed and running correctly by typing the following at the command line\.

   ```
   node -e "console.log('Running Node.js ' + process.version)"
   ```

   This should display the following message that confirms the installed version of Node\.js running\.

   `Running Node.js v8.11.3`

## Creating an Amazon Machine Image<a name="w3ab1c25b6b8"></a>

After you install Node\.js on an Amazon EC2 instance, you can create an Amazon Machine Image \(AMI\) from that instance\. Creating an AMI makes it easy to provision multiple Amazon EC2 instances with the same Node\.js installation\. For more information about creating an AMI from an existing instance, see [Creating an Amazon EBS\-Backed Linux AMI](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-an-ami-ebs.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Related Resources<a name="w3ab1c25b6c10"></a>

For more information about the commands and software used in this topic, see the following web pages:
+ node version manager \(nvm\): see [nvm repo on GitHub](https://github.com/creationix/nvm)\.
+ node package manager \(npm\): see [npm website](https://www.npmjs.com)\.
