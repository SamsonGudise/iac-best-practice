# Terraform Modules

Terrafrom modules provide a way to reuse code to build group of resouces such as EC2 Instances, AutoScalingGroup, ElasticsLoadBalancer and Route53 record to expore service to your customer.  You probably need to replicate infrastructure for multiple application or multiple environment such as Dev, QC and production. 

## Generics modules
Maintain balance between number of parameters to pass and genericness of the module. Start small with limited number of parameters and more parameters as needed. Keep module dry and use variables with default values. Variables with default values are optional parameters.

## Configurable modules 
Modules save lot of duplicate code, but it is exactly same for QC and production, you may want different configration such you ASG size or machine type.  Assign default values suitable for QC environment, pass specific values to production instances. 

You may also use `map` varaibles to set defaults values to both stage and proudction. 

## Output variables
Use module output variables to pass values between modules.  For example `subnet` module output may needed in `route-table` or vis-vera. 

## Versioned Modules
As soon as you make changes to modules they get deployed to all environments. Use versioned modules to test before it gets deployed ro production. You cab release verison 0.0.1, while working on version 0.0.2 

```
git tag -a "v0.0.1" -m "Initial release"
git push --follow-tags
```

```
module "vpc" {
  source = "git::git@github.com:terraform/terraform.git//network/vpc?ref=v0.0.1"
}
```
## Avoid inline blocks
Terraform resouces can be  defined as inline block or seperate resouces.  When creating modules,  you should always use seperate resource.

```
resource "aws_route_table" "example" {
  vpc_id = "${aws_vpc.example.id}"
  route {
    cidr_block = "10.0.1.0/24"
    gateway_id = "${aws_internet_gateway.example.id}"
  }
}
```

Alternactively, you can create exact same resouces as here 
```
resource "aws_route_table" "example" {
  vpc_id = "${aws_vpc.example.id}"
}
resource "aws_route" "example" {
  route_table_id = "${aws_route_table.example.id}"
  destination_cidr_block = "10.0.1.0/24"
  gateway_id = "${aws_internet_gateway.example.id}"
}
```
