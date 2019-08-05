# Work with Multiple Regions
Parameterize module with optional region parameter. i.e., define  region variable with default value as your popular region.
## Keep code dry from region specific data
We use s3bucket to save state info. Use dedicated s3bucket for each region and project. Use seperate `tfvars` file to pass region specific indformation.  

```
terraform init -backend-config=backend.tfvars
```
```
cat backend.tfvars
# s3 terraform state variables
bucket = "test-volvo-terraform-state"
```
`tfvars` file can also be use to pass `vpc_id`. Peering `vpc_id` might be different for each enviornment.

## DataSource
Use `DataSource` to query account/region specific information  instead of pass as parameters.