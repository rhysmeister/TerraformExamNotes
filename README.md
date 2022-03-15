# TerraformExamNotes
Notes for the Terraform Associate Certification

# Links

* [Study Guide](https://learn.hashicorp.com/tutorials/terraform/associate-study)
* [Sample Questions](https://learn.hashicorp.com/tutorials/terraform/associate-questions?in=terraform/certification)
* [Exam Review](https://learn.hashicorp.com/tutorials/terraform/associate-review?in=terraform/certification)
* [Terraform Associate Tutorials](https://learn.hashicorp.com/collections/terraform/certification-associate-tutorials)

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
  * 3a Handle Terraform and provider installation and versioning
    * [Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
      * Retrieve the terraform binary by downloading a pre-compiled binary or compiling it from source.
      * HashiCorp officially maintains and signs packages for the following Linux distributions.
        * Ubuntu/Debian
        * CentOS/RHEL
        * Fedora
        * Amazon Linux
    * [Terraform Providers](https://www.terraform.io/language/providers/configuration)
    * Providers allow Terraform to interact with cloud providers, SaaS providers, and other APIs.
    * [Terraform Provider Registry](https://registry.terraform.io/browse/providers)
    * A provider configuration is created in the root module using a `provider`block.
```terraform
provider "google" {
  project = "acme-app"
  region  = "us-central1"
}

# The default provider configuration; resources that begin with `aws_` will use
# it as the default, and it can be referenced as `aws`.
provider "aws" {
  region = "us-east-1"
}

# Additional provider configuration for west coast region; resources can
# reference this as `aws.west`.
provider "aws" {
  alias  = "west"
  region = "us-west-2"
}
```
    * The `terraform`configuration block is used to configure some behaviors of Terraform itself, such as requiring a minimum Terraform version to apply your configuration.
      * Each terraform block can contain a number of settings related to Terraform's behavior. 
      * Within a terraform block, only constant values can be used; arguments may not refer to named objects such as resources, input variables, etc
      * Can not use any of the Terraform language built-in functions.
      * The `terraform` block can contain the following keywords:
        * `cloud` - Integrate with [Terraform Cloud](https://www.terraform.io/language/settings/terraform-cloud)
```terraform
terraform {
  cloud {
    organization = "example_corp"

    workspaces {
      tags = ["app"]
    }
  }
}
````
        * `backend` - Configure a backend where state snapshots are stored. Cannot be used if the configuration also contains a `cloud` block.
        * [Terraform Backend](https://www.terraform.io/language/settings/backends/configuration)
```terraform
terraform {
  backend "s3" {
    bucket = "mybucket"
    key    = "path/to/my/key"
    region = "us-east-1"
  }
}
```      
        * `required_version` - setting accepts a [version constraint](https://www.terraform.io/language/expressions/version-constraints) string, which specifies which versions of Terraform can be used with your configuration.
        * `required_providers` - block specifies all of the providers required by the current module, mapping each local provider name to a source address and a version constraint.
```terraform
terraform {
  required_providers {
    aws = {
      version = "~> 2.13.0"
    }
    random = {
      version = ">= 2.1.2"
    }
  }

  required_version = "~> 0.12.29"
}
```
        * `experiments` - Enable experimental features in Terraform.
```terraform
terraform {
  experiments = [example]
}
```
        * `provider_meta` - This allows the provider to receive module-specific information, and is primarily intended for modules distributed by the same vendor as the associated provider.
```terraform
terraform {
  provider_meta "my-provider" {
    hello = "world"
  }
}
```
    * [Provider Versioning](https://learn.hashicorp.com/tutorials/terraform/provider-versioning)
      * terraform.lock.hcl
        * Created when you run `terraform init` for the first time.
        * Should be included in git to ensure other users of the code use the same provider versions.
```terraform
# This file is maintained automatically by "terraform init".
# Manual edits may be lost in future updates.

provider "registry.terraform.io/hashicorp/aws" {
  version     = "2.50.0"
  constraints = ">= 2.0.0"
  hashes = [
    "h1:aKw4NLrMEAflsl1OXCCz6Ewo4ay9dpgSpkNHujRXXO8=",
    ## ...
    "zh:fdeaf059f86d0ab59cf68ece2e8cec522b506c47e2cfca7ba6125b1cd06b8680",
  ]
}

provider "registry.terraform.io/hashicorp/random" {
  version     = "3.0.0"
  constraints = "3.0.0"
  hashes = [
    "h1:yhHJpb4IfQQfuio7qjUXuUFTU/s+ensuEpm23A+VWz0=",
    ## ...
    "zh:fbdd0684e62563d3ac33425b0ac9439d543a3942465f4b26582bcfabcb149515",
  ]
}
```
    * The `terraform init --upgrade` command will upgrade all providers to the latest version consistent within the version constraints previously established in your configuration.

  * 3b Describe plug-in based architecture
  * 3c Demonstrate using multiple providers
  * 3d Describe how Terraform finds and fetches providers
  * 3e Explain when to use and not use provisioners and when to use local-exec or remote-exec
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
    * Can be enabled by setting the `TF_LOG` envronment variable to any value.
    * Verbosity leveles:
      * TRACE
      * DEBUG
      * INFO
      * WARN
      * ERROR
    * Also vars `TF_LOG_CORE` and `TF_LOG_PROVIDER`environment variables available for finer grained control.
    * `TF_LOG_PATH`- Append output to a file.
* 5 - Interact with Terraform modules
  * 5a Contrast module source options
    * [Module Overview](https://learn.hashicorp.com/tutorials/terraform/module)
      * Organize configuration.
      * Encapsulate configuration.
      * Re-use configuration.
      * Provide consistency and ensure best practices.
    * [FInd and using modules](https://www.terraform.io/registry/modules/use)
    * [Terraform Registry](https://registry.terraform.io/)
* 6 - Navigate Terraform workflow
  * 6a Describe Terraform workflow ( Write -> Plan -> Create )
    * [The Core Terraform Workflow](https://www.terraform.io/intro/core-workflow)
      * Write - Author infrastructure as code.
      * Plan - Preview changes before applying.
      * Apply - Provision reproducible infrastructure.
    * [Terraform AWS Example](https://learn.hashicorp.com/tutorials/terraform/aws-build)
    * `terraform init`
      * Used to initialize a working directory containing Terraform configuration files.
      * This is the first command that should be run after writing a new Terraform configuration or cloning an existing one from version control. 
      * It is safe to run this command multiple times.
      * `terraform init --upgrade` - upgrade modules and plugins.
      * `terraform init -backend=false` - Initialize a project without accessing the configured backend.
    * `terraform validate`
      * command validates the configuration files in a directory.
      * Does not access any remote state or APIs.
      * Basically a syntax check.
      * Would be a good early step in a CI Pipeline.
      * `terraform validate -json` - Produce output in JSON format.
    * `terraform plan`
      * command creates an execution plan.
        * Reads the current state of any already-existing remote objects to make sure that the Terraform state is up-to-date.
        * Compares the current configuration to the prior state and noting any differences.
        * Proposes a set of change actions that should, if applied, make the remote objects match the configuration.
        * `terraform plan -destroy` - Deytroy all object, i.e. in a transient dev env.
        * `terraform plan -refresh-only` - Refresh the Terraform state, i.e. when you've made a change outside of Terraform.
        * You can disable a refresh with `terraform plan -refresh=false` useful when you need to make the process faster.
        * Replace a resource `terraform plan -replace=ADDRESS`.
        * `terraform plan -target=ADDRESS` - Focus only on a specific resource and dependent objects.
        * `terraform plan -var 'NAME=VALUE'` - Set the value of a variable. Can be used to set multiple variables. 
        * `terraform plan -var-file=FILENAME` - Sets values for variables from a tfvars file.
    * `terraform apply`
      * command executes the actions proposed in a Terraform plan.
      * `terraform apply` - No arguments, create a new plan and prompt to approve it.
      * `terraform apply myplan.plan` - Excute a plan created previously with `terraform plan -out myplan.plan`.
      * `terraform apply -auto-approve` - Skip approval of plan.
      * `terraform apply  -compact-warnings` - Less verbose error output.
      * `terraform apply -input=false` - Disable interactive prompts.
      * `terraform apply -json` - JSON output.
      * `terraform apply -lock=false` - No state lock, could be dangerous.
      * `terraform apply -lock-timout=DURATION` - instructs Terraform to retry acquiring a lock for a period of time before returning an error. The duration syntax is a number followed by a time unit letter, such as "3s" for three seconds.
      * `terraform apply parallelism=n` - Concurrent operations, default 10.
    * `terraform destroy`
      * a convenient way to destroy all remote objects managed by a particular Terraform configuration.
      * `terraform destroy` is an alias for `terraform apply -destroy`.
      * `terraform plan -destroy` - shows you the proposed destroy changes without executing them.
      * [AWS Destroy Example](https://learn.hashicorp.com/tutorials/terraform/aws-destroy)
* 7 - Implement and maintain state
  * 7a Describe default local backend
    * Backends define where Terraform's state snapshots are stored.
    * A given Terraform configuration can either specify a backend, integrate with Terraform Cloud, or do neither and default to storing state locally.
    * Terraform uses this persisted state data to keep track of the resources it manages.
    * By default, Terraform implicitly uses a backend called local to store state as a local file on disk.
    * [Terraform Backends](https://www.terraform.io/language/settings/backends)
    * [Local Backend configuration](https://www.terraform.io/language/settings/backends/local)
```terraform
terraform {
  backend "local" {
    path = "relative/path/to/terraform.tfstate"
  }
}
```
    * Data Source Configuration
```terraform
data "terraform_remote_state" "foo" {
  backend = "local"

  config = {
    path = "${path.module}/../../terraform.tfstate"
  }
}
```
  * 7b Outline state locking
    * [State Locking](https://www.terraform.io/language/state/locking)
      * If supported by your backend, Terraform will lock your state for all operations that could write state.
      * This prevents others from acquiring the lock and potentially corrupting your state.
      * If state locking fails, Terraform will not continue.
      * You can disable state locking for most commands with the `-lock` flag but it is not recommended.
      * Terraform has a `terraform force-unlock LOCK_ID` command to manually unlock state. Emergency use only, use with care!
      * [Force Unlock](https://www.terraform.io/cli/commands/force-unlock)
      * [Managing Terraform State Files](https://odysee.com/@thecloudcoach:5/managing-terraform-state-files-what-are:9)
  * 7c Handle backend authentication methods
    * [Backend Types](https://www.terraform.io/language/settings/backends)
      * local
      * remote
      * artifactory
        * [Artifactory Backend](https://www.terraform.io/language/settings/backends/artifactory)
        * Stores the state as an artifact in a given repository in Artifactory.
        * Generic HTTP repositories are supported, and state from different configurations may be kept at different subpaths within the repository.
        * This backend does not support state locking.
  ```terraform
  terraform {
  backend "artifactory" {
    username = "SheldonCooper"
    password = "AmyFarrahFowler"
    url      = "https://custom.artifactoryonline.com/artifactory"
    repo     = "foo"
    subpath  = "terraform-bar"
  }
}
```
```terraform
data "terraform_remote_state" "foo" {
  backend = "artifactory"
  config = {
    username = "SheldonCooper"
    password = "AmyFarrahFowler"
    url      = "https://custom.artifactoryonline.com/artifactory"
    repo     = "foo"
    subpath  = "terraform-bar"
  }
}
```
      * azurerm
      * consul
      * cos
      * etcd
      * etcdv3
      * gcs
      * http
        * [HTTP Backend](https://www.terraform.io/language/settings/backends/http)
```terraform
terraform {
  backend "http" {
    address = "http://myrest.api.com/foo"
    lock_address = "http://myrest.api.com/foo"
    unlock_address = "http://myrest.api.com/foo"
    username = user
    password = secret
  }
}
```
```terraform
data "terraform_remote_state" "foo" {
  backend = "http"
  config = {
    address = "http://my.rest.api.com"
    username = user
    password = secret
  }
}
```
      * Kubernetes
      * manta
      * oss
      * pg
      * s3
        * [S3 Backend](https://www.terraform.io/language/settings/backends/s3)
        * Stores the state as a given key in a given bucket on Amazon S3.
        * This backend also supports state locking and consistency checking via Dynamo DB.
```terraform
terraform {
  backend "s3" {
    bucket = "mybucket"
    key    = "path/to/my/key"
    region = "us-east-1"
    access_key = "XXXXXXXXXXXXXXXXX"
    secret_key = "XXXXXXXXXXXXXXXXX"
  }
}
```
      * swift
    * The built-in backends are the only backends. You cannot load additional backends as plugins.
  * 7d Describe remote state storage mechanisms and supported standard backends
    * Writes the state data to a remote data store, which can then be shared between all members of a team.
    * Supported Backends
      * Terraform Cloud
      * HashiCorp Consul
      * Amazon S3
      * Azure Blob Storage
      * Google Cloud Storage
      * Alibaba Cloud OSS
      * [More](https://www.terraform.io/language/settings/backends)
    * [Remote State](https://www.terraform.io/language/state/remote)
    * [Remote State Storage](https://learn.hashicorp.com/tutorials/terraform/aws-remote)
    * [Remote State and Backends](https://www.youtube.com/watch?v=RBW253A4SvY)
  * 7e 	Describe effect of Terraform refresh on state
    * `terraform refresh`reads the current settings from all managed remote objects and updates the Terraform state to match.
    * This won't modify your real remote objects, but it will modify the Terraform state.
    * A refresh is also performed when you do wither a `terraform plan` or `terraform apply`.
    * It's recommended not to use the `terraform refresh` command. [Read more](https://www.terraform.io/cli/commands/refresh)
    * [Manage Resource Drift](https://learn.hashicorp.com/tutorials/terraform/resource-drift)
    * [terraform plan --refresh-only](https://learn.hashicorp.com/tutorials/terraform/refresh)
  * 7f Describe backend block in configuration and best practices for partial configurations
    * A configuration can only provide one backend block.
    * A backend block cannot refer to named values (like input variables, locals, or data source attributes).
    * Backends are configured with a nested backend block within the top-level terraform block:
```terraform
terraform {
  backend "remote" {
    organization = "example_corp"

    workspaces {
      name = "my-app-prod"
    }
  }
}
```
    * It is recommended to not provide credentials in the backend configuration. These should be provided by secrets files or shell environment variables.
    * If no backend block is configured Terraform defaults to a local backend. This is fine for single-developer development use/training but anything more advanced should use a secure remote backend.
    * It is possible to migrate the state between backends.
    * Terraform state backups are recommended.
    * Partial Configuration
      * When some or all of the arguments are omitted, we call this a partial configuration.
      * With a partial configuration, the remaining configuration arguments must be provided as part of the initialization process.
      * There are several ways to provide the remaining arguments:
        * File - `terraform init -backend-config=PATH`
        * Command-line key/value pairs - `terraform init -backend-config="KEY=VALUE"`
        * Interactively - Terraform will prompt for required parameters only.
```terraform
terraform {
  backend "consul" {}
}
```
  * 7g Understand secret management in state files
    * [Sensitive Data in State](https://www.terraform.io/language/state/sensitive-data)
    * Terraform state files should be treated as sensitive data.
    * Use remote state that securely stores files, i.e. encryption at rest.
    * From Terraform 0.9 remote state is not persisted to disk and is only ever stored in memory.
    * Terraform Cloud
      * Encrypts at rest.
      * TLS in transit.
      * Identifies users.
      * State history.
      * Detailed auditing.
    * Amazon S3
      * Encryption option.
      * IAM Policies and logging for security and auditing.
      * Requests are TLS encrypted.
* 8 - Read, generate, and modify configuration
* 9 - Understand Terraform Cloud and Enterprise capabilities
  * [Migrate State to Terraform Cloud](https://learn.hashicorp.com/tutorials/terraform/cloud-migrate#set-up-the-remote-backend)
