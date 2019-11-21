# Terraform Provider - Active Directory

[![GolangCI](https://golangci.com/badges/github.com/golangci/golangci-lint.svg)](https://golangci.com)
![CircleCI](https://img.shields.io/circleci/build/github/adlerrobert/terraform-provider-activedirectory?style=flat-square&logo=circleci&label=CircleCI)
[![codecov](https://codecov.io/gh/adlerrobert/terraform-provider-activedirectory/branch/master/graph/badge.svg)](https://codecov.io/gh/adlerrobert/terraform-provider-activedirectory)
[![GitHub license](https://img.shields.io/github/license/adlerrobert/terraform-provider-activedirectory.svg?style=flat-square&cacheSeconds=3600)](https://github.com/adlerrobert/terraform-provider-activedirectory/blob/master/LICENSE)

[![GitHub release](https://img.shields.io/github/release/adlerrobert/terraform-provider-activedirectory.svg?style=flat-square)](https://GitHub.com/adlerrobert/terraform-provider-activedirectory/releases/)
[![GitHub tag](https://img.shields.io/github/tag/adlerrobert/terraform-provider-activedirectory.svg?style=flat-square)](https://github.com/adlerrobert/terraform-provider-activedirectory/tags/)

This is a Terraform  Provider to work with Active Directory.

This provider currently supports only computer objects, but more active directory resources are planned. Please feel free to contribute.

For general information about Terraform, visit the [official website][3] and the [GitHub project page][4].

[3]: https://terraform.io/
[4]: https://github.com/hashicorp/terraform

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) 0.12+
- [Go](https://golang.org/doc/install) 1.13 (to build the provider plugin)

## Developing the Provider
If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (please check the [requirements](https://github.com/adlerrobert/terraform-provider-activedirectory#requirements) before proceeding).

*Note:* This project uses [Go Modules](https://blog.golang.org/using-go-modules) making it safe to work with it outside of your existing [GOPATH](http://golang.org/doc/code.html#GOPATH). The instructions that follow assume a directory in your home directory outside of the standard GOPATH (i.e `$HOME/development/terraform-providers/`).

Clone repository to: `$HOME/development/terraform-providers/`

```sh
$ mkdir -p $HOME/development/terraform-providers/; cd $HOME/development/terraform-providers/
$ git clone git@github.com:adlerrobert/terraform-provider-activedirectory
...
```

Enter the provider directory and run `make tools`. This will install the needed tools for the provider.

```sh
$ make tools
```

To compile the provider, run `make build`. This will build the provider and put the provider binary in the `$GOPATH/bin` directory.

```sh
$ make build
...
$ $GOPATH/bin/terraform-provider-activedirectory
...
```

## Using the Provider
To use a released provider in your Terraform environment, run [`terraform init`](https://www.terraform.io/docs/commands/init.html) and Terraform will automatically install the provider. To specify a particular provider version when installing released providers, see the [Terraform documentation on provider versioning](https://www.terraform.io/docs/configuration/providers.html#version-provider-versions).

To instead use a custom-built provider in your Terraform environment (e.g. the provider binary from the build instructions above), follow the instructions to [install it as a plugin.](https://www.terraform.io/docs/plugins/basics.html#installing-a-plugin) After placing it into your plugins directory, run `terraform init` to initialize it.

The Active Directory provider is use to interact with Microsoft Active Directory. The provider needs to be configured with the proper credentials before it can be used.

Currently the provider only supports Active Directory Computer objects.

### Example
```hcl
# Configure the AD Provider
provider "activedirectory" {
  ad_host       = "ad.example.org"
  ad_port       = 389
  use_tls       = true
  bind_user     = "cn=admin,dc=example,dc=org"
  bind_password = "admin"
}

# Add computer to Active Directory
resource "activedirectory_computer" "foo" {
  name           = "TestComputerTF"                       # update will force destroy and new
  ou             = "CN=Computers,DC=example,DC=org"       # can be updated
  description    = "terraform sample server"              # can be updated
}
```

## Testing the Provider
In order to test the provider, you can run `make test`. This will run so-called unit tests.
```sh
$ make test
```

In order to run the full suite of Acceptance tests, run `make testacc`. Please make sure that a working Domain Controller is reachable and you have the needed permissions
*Note:* Acceptance tests create real resources! Please read [Running an Acceptance Test](https://github.com/adlerrobert/terraform-provider-axctivedirectory/blob/master/.github/CONTRIBUTING.md#running-an-acceptance-test) in the contribution guidelines for more information on usage.

```sh
$ make testacc
```

 For `make testacc` you have to set the following environment variables:

 | Variable | Description | Example | Default | Required |
 | -------- | ----------- | ------- | ------- | :------: |
 | AD_HOST | Domain Controller | dc.example.org | - | yes |
 | AD_PORT | LDAP Port - 389 TCP | 389 | 389 | no |
 | AD_USE_TLS | Use secure connection | false | true | no |
 | AD_BIND_USER | Admin user DN | cn=admin,dc=example,dc=org | - | yes |
 | AD_BIND_PASSWORD | Password of the admin user | secret | - | yes |
 | AD_COMPUTER_TEST_BASE_OU | OU for the test cases | ou=TerraformTests,dc=example,dc=org | yes (for tests) |

## Contributing
Terraform is the work of thousands of contributors. We appreciate your help!

To contribute, please read the contribution guidelines: [Contributing to Terraform - Active Directory Provider](.github/CONTRIBUTING.md)

Issues on GitHub are intended to be related to bugs or feature requests with provider codebase. See https://www.terraform.io/docs/extend/community/index.html for a list of community resources to ask questions about Terraform.
