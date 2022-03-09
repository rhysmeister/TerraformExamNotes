# TerraformExamNotes
Notes for the Terraform Associate Certification

# Links

* [Study Guide](https://learn.hashicorp.com/tutorials/terraform/associate-study)
* [Sample Questions](https://learn.hashicorp.com/tutorials/terraform/associate-questions?in=terraform/certification)
* [Exam Review](https://learn.hashicorp.com/tutorials/terraform/associate-review?in=terraform/certification)

# Review Guide

The notes here are a condensed version of the official Exam Review section linked above. That should be studied in detail and the notes here are intended only for quick review before the exam.

* 1 - Understand Infrastructure as Code (IaC) concepts
  * 1a - Explain What IaC is.
*** Infrastructure as code (IaC) is the process of managing and provisioning computer data centres through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.
  * 1b - Describe advantages of IaC patterns
*** Cost reduction - by removing the manual component, people can focus their efforts on other tasks, meaning costs are reduced.
*** Speed - Automation allows us to work faster, for example when a system is defined as code, we can recreate it when needed without all the manual work we used to do.
*** Risk - IaC removes the risk associated with human error, like manual misconfiguration, we reduce the chance of downtime and increase reliability.
* 2 - Understand Terraform's purpose (vs other IaC)
  * 2a - Explain multi-cloud and provider agnostic benefits
*** Spread infrastructure across multiple cloud providers to increase fault tolerance.
*** Terraform is able to handle multiple cloud providers so you do not have to learn and deploy multiple tools to support this.
  * 2b - Explain the benefits of state
*** Real world mapping of resources.
*** Track metadata of resources such as IP address of EC2 instance, its dependency with respect to other resources etc.
*** Cache resource attributes.
*** Enables collaboration among teams.
*** [Further Reading](https://medium.com/@mitesh_shamra/state-management-with-terraform-9f13497e54cf)
* 3 - Understand Terraform basics
* 4 - Use the Terraform CLI (outside of core workflow)
* 5 - Interact with Terraform modules
* 6 - Navigate Terraform workflow
* 7 - Implement and maintain state
* 8 - Read, generate, and modify configuration
* 9 - Understand Terraform Cloud and Enterprise capabilities
