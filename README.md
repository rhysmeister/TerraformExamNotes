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
    * Infrastructure as code (IaC) is the process of managing and provisioning computer data centres through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.
    * [https://en.wikipedia.org/wiki/Infrastructure_as_code](https://en.wikipedia.org/wiki/Infrastructure_as_code)
    * [What is Infrastructure As Code?](https://odysee.com/@thecloudcoach:5/what-is-infrastructure-as-code-terraform:5)
  * 1b - Describe advantages of IaC patterns
    * Cost reduction - by removing the manual component, people can focus their efforts on other tasks, meaning costs are reduced.
    * Speed - Automation allows us to work faster, for example when a system is defined as code, we can recreate it when needed without all the manual work we used to do.
    * Risk - IaC removes the risk associated with human error, like manual misconfiguration, we reduce the chance of downtime and increase reliability.
* 2 - Understand Terraform's purpose (vs other IaC)
  * 2a - Explain multi-cloud and provider agnostic benefits
    * Spread infrastructure across multiple cloud providers to increase fault tolerance.
    * Terraform is able to handle multiple cloud providers so you do not have to learn and deploy multiple tools to support this.
  * 2b - Explain the benefits of state
    * Real world mapping of resources.
    * Track metadata of resources such as IP address of EC2 instance, its dependency with respect to other resources etc.
    * Cache resource attributes.
    * Enables collaboration among teams.
    * [Further Reading](https://medium.com/@mitesh_shamra/state-management-with-terraform-9f13497e54cf)
* 3 - Understand Terraform basics
* 4 - Use the Terraform CLI (outside of core workflow)
  * 4a `terraform fmt` - command is used to rewrite Terraform configuration files to a canonical format and style. This command applies a subset of the Terraform language style conventions, along with other minor adjustments for readability.
    * [Terraform Sytle Conventions](https://www.terraform.io/language/syntax/style)
    * [Terraform fmt Video](https://www.youtube.com/watch?v=aPZ6a9QtgJw)
    * `terraform fmt --diff` - show what would be changed, without actually doing it.
  * 4b `terraform taint` - informs Terraform that a particular object has become degraded or damaged. Terraform represents this by marking the object as "tainted" in the Terraform state, and Terraform will propose to replace it in the next plan you create.
    * This command is deprecated. For Terraform v0.15.2 and later, use the -replace option with terraform apply instead.
    * Example cmd: `terraform taint aws_bucket.mybucket`
    * `terraform apply -replace="aws_instance.example[0]"`
    * [Taint and Untaint Resources](https://www.youtube.com/watch?v=ySGc_obU6wY)
  * 4c `terraform import` - Used to import existing resources into Terraform.
    * This command can be used to bring resources, that have been created outside of terraform, into management via Terraform.
    * Example cmd: `terraform import aws_instance.foo i-abcd1234`
    * [terraform import](https://www.youtube.com/watch?v=nvskcI-XPrY)
  * 4d `terraform workspace` - Can be used to store multiple environment states in a single backend. For example they can be used to implement dev, test, staging and prod environments using a single codebase.
    * [Terraform Workspaces](https://www.terraform.io/language/state/workspaces)
    * Example cmd: `terraform workspace new dev` - creates a new worspace called dev.
    * Example cmd: `terraform workspace select dev` - sets the current worksapce to dev.
    * We can reference the workspace in Terraform code with `${terraform.workspace}`.
    * Example code:
```terraform
resource "aws_instance" "example" {
  count = "${terraform.workspace == "default" ? 5 : 1}"

  # ... other arguments
}

resource "aws_instance" "example" {
  tags = {
    Name = "web - ${terraform.workspace}"
  }

  # ... other arguments
}
```
  * 4e `terraform state` - 
  * 4f Verbose logging 
* 5 - Interact with Terraform modules
* 6 - Navigate Terraform workflow
* 7 - Implement and maintain state
* 8 - Read, generate, and modify configuration
* 9 - Understand Terraform Cloud and Enterprise capabilities
