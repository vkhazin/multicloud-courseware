# Terraform Provider

* Responsible for interacting with provider-specific API's
* AWS:

```text
provider "aws" {
  version = "~> 2.0"
  region  = "us-east-1"
}
```

* Azure:

```text
provider "azurerm" {
  version = "=1.38.0"
}
```

* GCP:

```text
provider "google" {
  project     = "my-project-id"
  region      = "us-central1"
}
```

