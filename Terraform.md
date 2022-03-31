# Terraform

It is an open-source tool that lets you automate and manage your infrastructures, your platform, and your services. It is a declarative language.

## When to use Terraform

Let’s assume, you need to setup VM for your new project. Here are the steps:

1. Provisioning
    1. Create the private network space
    2. Create your instances
    3. Install docker and other tools you may need 
    4. Consider security (Like firewall)
2. Deploying applications

The Terraform comes for the first part of this scenario, Preparing the environment for deploying applications.

## Ansible VS Terraform

- Both are Infrastructure as a code: both used for provisioning, configuring, and managing the infrastructure.
- Terraform is mainly an infrastructure provisioning tool. It is relatively new, and it is more advanced in orchestration.
- Ansible is mainly a configuration tool.  Once the infrastructure is provisioned, Ansible is used for configuration, installing applications, and deploying applications.

## Benefits

- Managing existing infrastructures
- Replicating infrastructures

## Architecture

Core is a main component that takes the TF-Config and the current state of the infrastructure and then decide what and how to create/update/destroy.

The other main component of the Terraform is the Providers. Providers make us to communicate to cloud services such as AWS, Azure, or.

As it was mentioned it could be IaaS, PaaS, or even SaaS.

## Commands

- `refresh` to update current state of the infrastructure
- `plan` to make bunch of steps doing the changes until gets the final desired state.
- `apply` to execute the plan.

# References

[Terraform explained in 15 mins | Terraform Tutorial for Beginners](https://www.youtube.com/watch?v=l5k1ai_GBDE)