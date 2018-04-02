---
layout: "aws"
page_title: "AWS: aws_service_discovery_service"
sidebar_current: "docs-aws-resource-service-discovery-service"
description: |-
  Provides a Service Discovery Service resource.
---

# aws_service_discovery_service

Provides a Service Discovery Service resource.

## Example Usage

```hcl
resource "aws_vpc" "example" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_service_discovery_private_dns_namespace" "example" {
  name = "example.terraform.local"
  description = "example"
  vpc = "${aws_vpc.example.id}"
}

resource "aws_service_discovery_service" "example" {
  name = "example"
  dns_config {
    namespace_id = "${aws_service_discovery_private_dns_namespace.example.id}"
    dns_records {
      ttl = 10
      type = "A"
    }
    routing_policy = "MULTIVALUE"
  }
}
```

```hcl
resource "aws_service_discovery_public_dns_namespace" "example" {
  name = "example.terraform.com"
  description = "example"
}

resource "aws_service_discovery_service" "example" {
  name = "example"
  dns_config {
    namespace_id = "${aws_service_discovery_public_dns_namespace.example.id}"
    dns_records {
      ttl = 10
      type = "A"
    }
  }
  health_check_config {
    failure_threshold = 100
    resource_path = "path"
    type = "HTTP"
  }
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required, ForceNew) The name of the service.
* `description` - (Optional) The description of the service.
* `dns_config` - (Required) A complex type that contains information about the resource record sets that you want Amazon Route 53 to create when you register an instance.
* `health_check_config` - (Optional) A complex type that contains settings for an optional health check. Only for Public DNS namespaces.

### dns_config

The following arguments are supported:

* `namespace_id` - (Required, ForceNew) The ID of the namespace to use for DNS configuration.
* `dns_records` - (Required) An array that contains one DnsRecord object for each resource record set.
* `routing_policy` - (Optional) The routing policy that you want to apply to all records that Route 53 creates when you register an instance and specify the service. Valid Values: MULTIVALUE, WEIGHTED

#### dns_records

The following arguments are supported:

* `ttl` - (Required) The amount of time, in seconds, that you want DNS resolvers to cache the settings for this resource record set.
* `type` - (Required, ForceNew) The type of the resource, which indicates the value that Amazon Route 53 returns in response to DNS queries. Valid Values: A, AAAA, SRV, CNAME

### health_check_config

The following arguments are supported:

* `failure_threshold` - (Optional) The number of consecutive health checks. Maximum value of 10.
* `resource_path` - (Optional) An array that contains one DnsRecord object for each resource record set.
* `type` - (Optional, ForceNew) An array that contains one DnsRecord object for each resource record set.

## Attributes Reference

The following attributes are exported:

* `id` - The ID of the service.
* `arn` - The ARN of the service.

## Import

Service Discovery Service can be imported using the service ID, e.g.

```
$ terraform import aws_service_discovery_service.example 0123456789
```
