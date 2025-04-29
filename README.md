# Terraform Helm Module

This Terraform module provides a standardized way to deploy Helm releases in Kubernetes clusters. It wraps the Helm provider functionality and provides a consistent interface for deploying Helm charts with extensive configuration options.

## Features

- Flexible chart deployment with support for local charts, remote repositories, and URLs
- Comprehensive configuration options for Helm releases
- Namespace management
- Support for custom values and sensitive data
- Advanced Helm features like atomic deployments, hooks, and post-render operations

## Usage

```hcl
module "helm_release" {
  source = "path/to/terraform-aws-helm"

  # Required
  name  = "my-release"
  chart = "my-chart"

  # Optional
  namespace        = "my-namespace"
  create_namespace = true
  repository       = "https://charts.example.com"
  chart_version    = "1.0.0"
  
  values = [
    file("values.yaml")
  ]

  # Additional configuration options
  timeout     = 300
  atomic      = true
  wait        = true
  max_history = 5
}
```

## Requirements

- Terraform >= 0.13.x
- Helm Provider
- Kubernetes cluster access configured

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| create | Controls if resources should be created | `bool` | `true` | no |
| create_release | Determines whether the Helm release is created | `bool` | `true` | no |
| name | Name of the Helm release | `string` | `""` | yes |
| chart | Chart name to be installed | `string` | `""` | yes |
| namespace | The namespace to install the release into | `string` | `null` | no |
| create_namespace | Create the namespace if it does not exist | `bool` | `null` | no |
| chart_version | Specify the exact chart version to install | `string` | `null` | no |
| repository | Repository URL where to locate the requested chart | `string` | `null` | no |
| values | List of values in raw yaml to pass to helm | `list(string)` | `null` | no |

Additional configuration options are available - see variables.tf for full details.

## Advanced Features

### Sensitive Values

Use `set_sensitive` for handling sensitive data:

```hcl
module "helm_release" {
  # ... other configuration ...

  set_sensitive = [
    {
      name  = "credentials.password"
      value = var.sensitive_password
    }
  ]
}
```

### Post-Render Operations

Configure post-render operations to modify manifests:

```hcl
module "helm_release" {
  # ... other configuration ...

  postrender = {
    binary_path = "/path/to/binary"
    args        = ["arg1", "arg2"]
  }
}
```

## Best Practices

1. Always specify a chart version for production deployments
2. Use `wait = true` and `atomic = true` for critical deployments
3. Configure appropriate timeout values for large deployments
4. Use `set_sensitive` for handling secrets and sensitive data
5. Implement proper release history limits using `max_history`

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This module is released under the MIT License.
