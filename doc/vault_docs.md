# Vault Documentation

## Concepts

### Policies

Everything in Vault is path based, and policies are no exception.
Policies provide a declarative way to grant or forbid access to certain paths and operations in Vault.
This section discusses policy workflows and syntaxes.

Policies are **deny by default**, so an empty policy grants no permission in the system.

#### Policy-Authorization Workflow

Before a human or machine can gain access, an administrator must configure Vault with an auth method.
Authentication is the process by which human or machine-supplied information is verified against an internal or external system.

#### Policy Syntax

Policies are written in HCL or JSON and describe which paths in Vault a user or machine is allowed to access.

Here is a very simple policy which grants read capabilities to the path "`secret/foo`":
```hcl
path "secret/foo" {
  capabilities = ["read"]
}
```
When this policy is assigned to a token, the token can read from "`secret/foo`".
However, the token cannot update or delete "`secret/foo`", since the capabilities do not allow it.
Because policies are deny by default, the token would have no other access in Vault.

## Auth Methods

Auth methods are the components in Vault that perform authentication and are responsible for assigning identity and a set of policies to a user.

Having multiple auth methods enables you to use an auth method that makes the most sense for your use case of Vault and your organization.

### AWS Auth Method

The `aws` auth method provides an automated mechanism to retrieve a Vault token for IAM principals and AWS EC2 instances.
Unlike most Vault auth methods, this method does not require manual first-deploying, or provisioning security-sensitive credentials (tokens, username/password, client certificates, etc), by operators under many circumstances.

### Authentication Workflow

There are two authentication types present in the aws auth method: `iam` and `ec2`.
