# Create vpc
Name : my vpc
IPV$ CIDR Block: 10.0.0.0/16

# create public subnets

Name: public 1a
availability zone: us-east-1a
IPv4 CIDR Block: 10.0.1.0/24

Name: public 1b
availability zone: us-east-1b
IPv4 cidr block: 10.0.2.0/24

Name: private 1a
availability zone: us-east-1b
IPv4 cidr block: 10.0.2.0/24

Name: private 1b
availability zone: us-east-1b
IPv4 cidr block: 10.0.2.0/24

# create private route table

name; private-RT
VPC : my Vpc 
subnet associations: private-1a, private-1b

# create Internet Gateway

name: myIGW
VPC : my VPC


