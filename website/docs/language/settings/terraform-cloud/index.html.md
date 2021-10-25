---
layout: "language"
page_title: "Terraform Cloud Overview"
sidebar_current: "terraform-cloud-configuration"
description: "Configure Terraform to use Terraform Cloud"
---

# Using Terraform Cloud with Terraform

Terraform Cloud can be used on the command line with Terraform itself. Using Terraform Cloud in this
way is referred to as the [CLI-driven run workflow](/docs/cloud/run/cli.html).

Operations like `terraform plan` or `terraform apply` are remotely executed in Terraform Cloud's run
environment, with log output streaming to the local terminal. This allows usage of Terraform Cloud's
features - things like using variables encrypted at rest in a Terraform Cloud workspace, cost
estimates, and policy checking - into the familiar Terraform CLI workflow.

Workspaces can also be configured for local execution, in which case only state is stored in
Terraform Cloud. In this mode, Terraform Cloud behaves just like a standard state backend.

-> **Note:** The Cloud integration for Terraform was added in Terraform 1.1.0; for previous
versions, see the [remote backend](/docs/language/settings/backends/remote.html).

-> **Note:** This integration supports Terraform Enterprise as well. Throughout all the
documentation, the platform will be referred to as Terraform Cloud, with any Terraform
Enterprise-specific details explicitly stated. The minimum required version of Terraform Enterprise
is <TODO>.

### Excluding Files from Upload with .terraformignore

When executing a remote `plan` or `apply` in a [CLI-driven run](/docs/cloud/run/cli.html),
an archive of your configuration directory is uploaded to Terraform Cloud. You can define
paths to ignore from upload via a `.terraformignore` file at the root of your configuration directory. If this file is not present, the archive will exclude the following by default:

* .git/ directories
* .terraform/ directories (exclusive of .terraform/modules)

The `.terraformignore` file can include rules as one would include in a
[.gitignore file](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository#_ignoring)


* Comments (starting with `#`) or blank lines are ignored
* End a pattern with a forward slash / to specify a directory
* Negate a pattern by starting it with an exclamation point `!`

Note that unlike `.gitignore`, only the `.terraformignore` at the root of the configuration
directory is considered.
