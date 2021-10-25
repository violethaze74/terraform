---
layout: "language"
page_title: "Workspaces"
sidebar_current: "terraform-cloud-configuration"
description: "About Terraform and Terraform Cloud Workspaces"
---

# Workspaces

Terraform can be configured to work with multiple Terraform Cloud workspaces using [Terraform's named workspaces feature](/docs/language/settings/terraform-cloud/workspaces.html) or a single explicit Terraform workspace.

(Explain org bucket here)

The `workspaces` block of the backend configuration determines which mode it uses:

- To use multiple remote workspaces, set `workspaces.prefix` to a prefix used in
  all of the desired remote workspace names. For example, set
  `prefix = "networking-"` to use Terraform cloud workspaces with
  names like `networking-dev` and `networking-prod`. This is helpful when
  mapping multiple Terraform CLI [workspaces](/docs/language/state/workspaces.html)
  used in a single Terraform configuration to multiple Terraform Cloud
  workspaces.
  o
- To use a single remote Terraform Cloud workspace, set `workspaces.name` to the
  remote workspace's full name (like `networking`).

The backend configuration requires either `name` or `prefix`. Omitting both or
setting both results in a configuration error.

If previous state is present when you run `terraform init` and the corresponding
remote workspaces are empty or absent, Terraform will create workspaces and/or
update the remote state accordingly. However, if your workspace needs variables
set or requires a specific version of Terraform for remote operations, we
recommend that you create your remote workspaces on Terraform Cloud before
running any remote operations against them.

