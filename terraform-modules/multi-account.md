# Work with Multiple AWS Accounts
Global variables such as regions, VPC_ID are used by `DataSource` to query resouces.  Maintain one source of truth for global variables.

Use multiple provider defintion to access two more accounts. For example you may have to deal with multiple accounts to establish VPC peering or  enable Custom Gateways. 

```
provider "aws" {
  alias  = "peer"
  region = "${var.peer_region}"
  # Accepter's credentials.
}

data "aws_caller_identity" "peer" {
  provider = "aws.peer"
}
```