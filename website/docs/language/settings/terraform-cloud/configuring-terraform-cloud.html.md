---
layout: "language"
page_title: "Configuring Terraaform Cloud"
sidebar_current: "terraform-cloud-configuration"
description: "Configuring Terraform Cloud"
---

# Configuring Terraform Cloud

To enable the [CLI-driven run workflow](https://www.terraform.io/docs/cloud/run/cli.html), a
Terraform configuration can integrate Terraform Cloud via a special `cloud` block within the
top-level `terraform` block, e.g.:

```
terraform {
  cloud {
    organization = "my-org"
    workspaces {
      tags = ["networking"]
    }
  }
}
```

Using the Cloud integration is mutually exclusive of declaring any backend; that is, a configuration
can only declare one or the other. Similar to backends...

- A configuration can only provide one cloud block.
- A cloud block cannot refer to named values (like input variables, locals, or data source attributes).

## Configuration variables

The following configuration options are supported:

* `hostname` - (Optional) The hostname of a Terraform Enterprise installation, if using Terraform
  Enterprise. Defaults to Terraform Cloud (app.terraform.io).
* `organization` - (Required) The name of the organization containing the
  workspace(s) the current configuration should be mapped to.
* `token` - (Optional) The token used to authenticate with Terraform Cloud.
  We recommend omitting the token from the configuration, and instead using
  [`terraform login`](/docs/cli/commands/login.html) or manually configuring
  `credentials` in the
  [CLI config file](/docs/cli/config/config-file.html#credentials).
* `workspaces` - (Required) A block declaring a strategy for mapping local CLI workspaces to remote
  Terraform Cloud workspaces.
  The `workspaces` block supports the following keys, each denoting a 'strategy':

  * `tags` - (Optional) TODO

  * `name` - (Optional) The full name of one remote workspace. When configured,
    only the default workspace can be used. This option conflicts with `prefix`.

    For more details on using workspaces, this configuration block, and mapping strategies, see [Workspaces](/docs/language/settings/terraform-cloud/workspaces.html)

## Example Configurations

->  **Note:** We recommend omitting the token from the configuration, and instead using
  [`terraform login`](/docs/cli/commands/login.html) or manually configuring
  `credentials` in the [CLI config file](/docs/cli/config/config-file.html#credentials)init.

### Basic Configuration

```hcl
# Using a single workspace:
terraform {
  backend "remote" {
    hostname = "app.terraform.io"
    organization = "company"

    workspaces {
      name = "my-app-prod"
    }
  }
}

# Using multiple workspaces:
terraform {
  backend "remote" {
    hostname = "app.terraform.io"
    organization = "company"

    workspaces {
      prefix = "my-app-"
    }
  }
}
```

### Using CLI Input

```hcl
# main.tf
terraform {
  required_version = "~> 0.12.0"

  backend "remote" {}
}
```

Backend configuration file:

```hcl
# config.remote.tfbackend
workspaces { name = "workspace" }
hostname     = "app.terraform.io"
organization = "company"
```

Running `terraform init` with the backend file:

```sh
terraform init -backend-config=config.remote.tfbackend
```

