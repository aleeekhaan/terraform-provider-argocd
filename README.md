<a href="https://terraform.io">
    <img src=".github/tf.png" alt="Terraform logo" title="Terraform" align="left" height="50" />
</a>

<a href="https://argoproj.github.io/cd">
    <img src=".github/argo-cd.png" alt="Terraform logo" title="Terraform" align="right" height="50" />
</a>

# Terraform Provider for ArgoCD

[![Tests](https://github.com/oboukili/terraform-provider-argocd/actions/workflows/tests.yml/badge.svg)](https://github.com/oboukili/terraform-provider-argocd/actions/workflows/tests.yml)

The [ArgoCD Terraform
Provider](https://registry.terraform.io/providers/oboukili/argocd/latest/docs)
provides lifecycle management of
[ArgoCD](https://argo-cd.readthedocs.io/en/stable/) resources.

**NB**: The provider is not concerned with the installation/configuration of
ArgoCD itself. To make use of the provider, you will need to have an existing
ArgoCD deployment and, the ArgoCD API server must be
[accessible](https://argo-cd.readthedocs.io/en/stable/getting_started/#3-access-the-argo-cd-api-server)
from where you are running Terraform.

---

## Documentation

Official documentation on how to use this provider can be found on the
[Terraform
Registry](https://registry.terraform.io/providers/oboukili/argocd/latest/docs).

## Compatibility promise

This provider is compatible with _at least_ the last 2 minor releases of ArgoCD
(e.g, ranging from 1.(n).m, to 1.(n-1).0, where `n` is the latest available
minor version).

Older releases are not supported and some resources may not work as expected.

## Motivations

### *I thought ArgoCD already allowed for 100% declarative configuration?*

While that is true through the use of ArgoCD Kubernetes Custom Resources, there
are some resources that simply cannot be managed using Kubernetes manifests,
such as project roles JWTs whose respective lifecycles are better handled by a
tool like Terraform. Even more so when you need to export these JWTs to another
external system using Terraform, like a CI platform.

### *Wouldn't using a Kubernetes provider to handle ArgoCD configuration be enough?*

Existing Kubernetes providers do not patch arrays of objects, losing project
role JWTs when doing small project changes just happen.

ArgoCD Kubernetes admission webhook controller is not as exhaustive as ArgoCD
API validation, this can be seen with RBAC policies, where no validation occur
when creating/patching a project.

Using Terraform to manage Kubernetes Custom Resource becomes increasingly
difficult the further you use HCL2 DSL to merge different data structures *and*
want to preserve type safety.

Whatever the Kubernetes CRD provider you are using, you will probably end up
using `locals` and the `yamlencode` function **which does not preserve the
values' type**. In these cases, not only the readability of your Terraform plan
will worsen, but you will also be losing some safeties that Terraform provides
in the process.

---

## Requirements

* [Terraform](https://www.terraform.io/downloads) (>= 1.0)
* [Go](https://go.dev/doc/install) (1.19)
* [GNU Make](https://www.gnu.org/software/make/)
* [golangci-lint](https://golangci-lint.run/usage/install/#local-installation) (optional)

---

## Credits

* Thanks to [JetBrains](https://www.jetbrains.com/?from=terraform-provider-argocd) for providing a GoLand open source license to support the development of this provider.
* Thanks to [Keplr](https://www.welcometothejungle.com/fr/companies/keplr) for allowing me to contribute to this side-project of mine during paid work hours.

![](sponsors/jetbrains.svg?display=inline-block) ![](sponsors/keplr.png?display=inline-block)
