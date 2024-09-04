# Infra Optimization
# Course-end Project 1


## Description

Create a DevOps infrastructure for an e-commerce application to run on high-availability mode.

## Background of the problem statement:

A popular payment application, EasyPay where users add money to their wallet accounts, faces an issue in its payment success rate. The timeout that occurs with
the connectivity of the database has been the reason for the issue.

While troubleshooting, it is found that the database server has several downtime instances at irregular intervals. This situation compels the company to create their own infrastructure that runs in high-availability mode.
Given that online shopping experiences continue to evolve as per customer expectations, the developers are driven to make their app more reliable, fast, and secure for improving the performance of the current system.

## Implementation requirements:

- Create the cluster (EC2 instances with load balancer and elastic IP in case of AWS)
- Automate the provisioning of an EC2 instance using Ansible or Chef Puppet
- Install Docker and Kubernetes on the cluster
- Implement the network policies at the database pod to allow ingress traffic from the front-end application pod
- Create a new user with permissions to create, list, get, update, and delete pods
- Configure application on the pod
- Take snapshot of ETCD database
- Set criteria such that if the memory of CPU goes beyond 50%, environments automatically get scaled up and configured

## The following tools must be used:

- EC2
- Kubernetes
- Docker
- Ansible or Chef or Puppet

## The following things to be kept in check:

- You need to document the steps and write the algorithms in them.
- The submission of your GitHub repository link is mandatory. In order to track your tasks, you need to share the link of the repository.
- Document the step-by-step process starting from creating test cases, then executing them, and recording the results.

## You need to submit the final specification document, which includes:

- Project and tester details
- Concepts used in the project
- Links to the GitHub repository to verify the project completion
- Your conclusion on enhancing the application and defining the USPs (Unique Selling Points)

## Solution:

