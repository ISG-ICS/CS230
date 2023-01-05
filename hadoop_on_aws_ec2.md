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
   - On Linux/Mac, 
```bash
ssh –i <pem_filepath> ubuntu@<public_ip>

# `<pem_filepath>` is the path to the `.pem` file downloaded in Step 1, 
# `<public_ip>` is the `Public IPv4 address` in the details of each instance.
# NOTE: If see `Permission denied (publickey).` error, 
#       use `chmod 400 <pem_filepath>` to update the permissions to the `.pem` file.
```

## Step 3 Java Installation
(On all 3 instances)
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

## Step 4 Setting up password-less SSH login between instances
 - Assign names to the 3 instances: `Master`, `Worker1`, `Worker2`.
 - Generate the public key and private key on all 3 instances.
```bash
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
```
 - Show the public key on all 3 instances.
```bash
cat .ssh/id_rsa.pub
```
 - Copy and append all 3 public keys to all 3 instances' `.ssh/authorized_keys` file.
 - Verify that you can ssh login in all the following directions without password.
On `Master` run the following,
```bash
# From Master to Master
ssh <public-ip-of-master>
ssh <private-ip-of-master>

# From Master to Worker1
ssh <public-ip-of-worker1>
ssh <private-ip-of-worker1>

# From Master to Worker2
ssh <public-ip-of-worker2>
ssh <private-ip-of-worker2>
```
On `Worker1` run the following,
```bash
# From Worker1 to Worker1
ssh <public-ip-of-worker1>
ssh <private-ip-of-worker1>

# From Worker1 to Master
ssh <public-ip-of-master>
ssh <private-ip-of-master>
```
On `Worker2` run the following,
```bash
# From Worker2 to Worker2
ssh <public-ip-of-worker2>
ssh <private-ip-of-worker2>

# From Worker2 to Master
ssh <public-ip-of-master>
ssh <private-ip-of-master>
```
