# Deploy Hadoop on AWS EC2
## Prerequisites
 - An AWS account
## Step 1 Create a key pair
 - Go to https://aws.amazon.com/ -> `Sign In to the Console`.
 - `Sevices` -> `Compute` -> `EC2`.
 - Left menu -> `Network & Security` -> `Key Pairs`.
 - `Create key pair`
   - Input `Name`, e.g., `cs230`.
   - Select `Private key file format`, use `.pem` for Linux/Mac or `.ppk` for Windows.
   - Confirm `Create key pair`.
   - Save the file to your local computer for future use.
## Step 2 Create 3 Amazon EC2 Ubuntu instances
 - Left menu -> `Instances` -> `Instances`.
 - `Launch instances`
   - Under tab `Application and OS Images (Amazon Machine Image)`, select `Ubuntu`. (By default, the AMI is `Ubuntu Server 22.04 LTS (HVM), SSD Volume Type`)
   - Under tab `Instance type`, select `t2.micro` (by default).
   - Under tab `Key pair`, select the one created in Step 1.
   - On the right `Summary`, select `3` in `Number of instances`, then `Launch instance`.
   - Wait for the 3 instances' statuses become `running`.
 - Use terminal to connect to the instances.
   - In the following, 
     - `<pem_filepath>` is the path to the `.pem` file downloaded in Step 1, 
     - `<public_ip>` is the `Public IPv4 address` in the details of each instance.
   - From Linux/Mac, `ssh –i <pem_filepath> ubuntu@<public_ip>`.
     - If see `Permission denied (publickey).` error, use `chmod 400 <pem_filepath>` to update the permissions to the `.pem` file.
## Step 3 Java Installation
 - Install Java and verify installation using the following commands.
```bash
$ sudo apt update
$ sudo apt-get install default-jre
$ java -version
```
 - Set up the `JAVA_HOME` environment variable.
Modify the environment file
```bash
sudo vi /etc/environment
## Add the following line to the file and save
JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
```
Load the environment file in the current session
```bash
source /etc/environment
```
Verify the `JAVA_HOME` variable has value.
```bash
echo $JAVA_HOME
```
