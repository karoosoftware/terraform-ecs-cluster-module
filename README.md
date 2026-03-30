# AWS ECS Cluster Module

This module creates a single AWS ECS cluster that can be shared by ECS workloads such as scheduled tasks and services.

## What This Module Creates

- 1 ECS cluster

## Usage

```hcl
module "ecs_cluster" {
  source = "git::ssh://git@github.com:karoosoftware/terraform-ecs-cluster-module.git?ref=<commit-sha>"

  name = "margana"

  tags = {
    Environment = "preprod"
    Application = "Margana"
  }
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| `name` | Name of the ECS cluster. | `string` | n/a | yes |
| `tags` | Tags to apply to the ECS cluster. | `map(string)` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| `cluster_arn` | ARN of the ECS cluster |
| `cluster_id` | ID of the ECS cluster |
| `cluster_name` | Name of the ECS cluster |

## Notes

- This module intentionally creates only the ECS cluster.
- Task definitions, services, scheduled tasks, and supporting networking should be managed by separate modules.
- This module can be used both for new cluster creation and for importing an existing ECS cluster into Terraform state.

## Release Process

- Update the root `VERSION` file in the same change that should be released, using semantic versioning such as `1.0.1`, `1.1.0`, or `2.0.0`.
- Push the change to `develop` and let the `terraform-validate` workflow pass.
- Open a pull request from `develop` to `main` and let the `terraform-validate` workflow pass again.
- Merge the pull request to `main`.
- Pushing to `main` triggers the automated release workflow, which:
  - reads `VERSION`,
  - checks that tag `v<VERSION>` does not already exist,
  - creates and pushes the tag,
  - creates the GitHub release automatically.
- If `VERSION` has not been updated and the tag already exists, validation and release will fail.
- Consume released versions from other Terraform repos by pinning the module source with the released tag, for example:

```bash
source = "git::ssh://git@github.com:karoosoftware/terraform-ecs-cluster-module.git?ref=v1.0.0"
```

## Prerequisites

- Terraform 1.x
- AWS provider configured in the root module
- IAM permissions to create or import ECS clusters
