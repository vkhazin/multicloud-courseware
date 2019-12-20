# Terraform Provider

* Responsible for interacting with provider-specific API's
* AWS:

```
provider "aws" {
  version = "~> 2.0"
  region  = "us-east-1"
}
```

* Azure:

```
provider "azurerm" {
  version = "=1.38.0"
}
```

* GCP:

```
provider "google" {
  project     = "my-project-id"
  region      = "us-central1"
}
```



