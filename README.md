# awssh

A very simply SSH wrapper aimed at making it easier to connect to AWS EC2 instances, written in BASH. You can SSH to EC2 instances by using either the instance ID or the 'Name' tag using either the public or private IP and optionally a bastion host. When using the public IP the script will authorize ingress security groups for the connecting IP and revoke the rule when complete.  

## Installation

1. Copy `awssh` to your preferred bin folder (or add it to your `$PATH`)
2. (Optional) Update `~/awssh.conf` with preferred defaults to reduce require options at runtime

### Prerequisites
1. AWS CLI installed
2. jq

## Usage
Usage: /usr/local/bin/awssh [-r region_name] [-u ssh_user] [-k private key name] [-p private IP] [-b <bastion>] [-B] <instance name|id|list>
-r region
-u SSH username
-k path to private key
-b bastion host specified
-B bastion host configured with env variable or via awssh.conf
-P AWS CLI profile to use - defaults to default
-c command to execute

list will provide a tabulated list of name|instance ID|region for the given parameters

Default values can be sourced from ~/awssh.conf


`region` is mandatory (required to get relevant IP address). If ssh_user is not defined the default value `ec2-user` is used. 

* `awssh i-1234567890abcdef0`

SSH to the specified instance - looks up the public IP address using the AWS CLI. Reads region information from ~/awssh.conf - you can define a path to a private explicitly as well otherwise the default ssh authentication agent will be used (e.g., via ssh-add)

* `awssh i-1234567890abcdef0 -r us-west-2 -u ubuntu`

Options passed at runtime will override variables defined in ~/awssh.conf

* `awssh i-1234567890abcdef0 -p -B`

Connect to the instance using the private IP address and the bastion host defined by `${bastion}`

* `awssh i-1234567890abcdef0 -p -b <IP of bastion host>`

Connect to the instance using the private IP address and the bastion host passed as option argument. 

## Why?
I spin-up a lot of short-lived, non-production environments in various configurations and this is just a quick and easy way to connect to them when necessary. 