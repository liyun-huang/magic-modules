---
subcategory: "Compute Engine"
description: |-
  Manages a network peering within GCE.
---

# google_compute_network_peering

Manages a network peering within GCE. For more information see
[the official documentation](https://cloud.google.com/compute/docs/vpc/vpc-peering)
and
[API](https://cloud.google.com/compute/docs/reference/latest/networks).

-> Both networks must create a peering with each other for the peering
to be functional.

~> Subnets IP ranges across peered VPC networks cannot overlap.

## Example Usage

```hcl
resource "google_compute_network_peering" "peering1" {
  name         = "peering1"
  network      = google_compute_network.default.self_link
  peer_network = google_compute_network.other.self_link
}

resource "google_compute_network_peering" "peering2" {
  name         = "peering2"
  network      = google_compute_network.other.self_link
  peer_network = google_compute_network.default.self_link
}

resource "google_compute_network" "default" {
  name                    = "foobar"
  auto_create_subnetworks = "false"
}

resource "google_compute_network" "other" {
  name                    = "other"
  auto_create_subnetworks = "false"
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) Name of the peering.

* `network` - (Required) The primary network of the peering.

* `peer_network` - (Required) The peer network in the peering. The peer network
may belong to a different project.

* `export_custom_routes` - (Optional)
Whether to export the custom routes to the peer network. Defaults to `false`.

* `import_custom_routes` - (Optional)
Whether to import the custom routes from the peer network. Defaults to `false`.

* `export_subnet_routes_with_public_ip` - (Optional)
Whether subnet routes with public IP range are exported. The default value is true, all subnet routes are exported. The IPv4 special-use ranges (https://en.wikipedia.org/wiki/IPv4#Special_addresses) are always exported to peers and are not controlled by this field.

* `import_subnet_routes_with_public_ip` - (Optional)
Whether subnet routes with public IP range are imported. The default value is false. The IPv4 special-use ranges (https://en.wikipedia.org/wiki/IPv4#Special_addresses) are always imported from peers and are not controlled by this field.

* `stack_type` - (Optional)
Which IP version(s) of traffic and routes are allowed to be imported or exported between peer networks. The default value is IPV4_ONLY. Possible values: ["IPV4_ONLY", "IPV4_IPV6"].

* `update_strategy` - (Optional, [Beta](https://terraform.io/docs/providers/google/guides/provider_versions.html))
The update strategy determines the semantics for updates and deletes to the peering connection configuration. The default value is INDEPENDENT. Possible values: ["INDEPENDENT", "CONSENSUS"]

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are
exported:

* `id` - an identifier for the resource with format `{{network}}/{{name}}`

* `state` - State for the peering, either `ACTIVE` or `INACTIVE`. The peering is
`ACTIVE` when there's a matching configuration in the peer network.

* `state_details` - Details about the current state of the peering.

## Timeouts

This resource provides the following
[Timeouts](https://developer.hashicorp.com/terraform/plugin/sdkv2/resources/retries-and-customizable-timeouts) configuration options: configuration options:

- `create` - Default is 4 minutes.
- `delete` - Default is 4 minutes.

## Import

VPC network peerings can be imported using the name and project of the primary network the peering exists in and the name of the network peering

* `{{project_id}}/{{network_id}}/{{peering_id}}`

In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import VPC network peerings using one of the formats above. For example:

```tf
import {
  id = "{{project_id}}/{{network_id}}/{{peering_id}}"
  to = google_compute_network_peering.default
}
```

When using the [`terraform import` command](https://developer.hashicorp.com/terraform/cli/commands/import), VPC network peerings can be imported using one of the formats above. For example:

```
$ terraform import google_compute_network_peering.default {{project_id}}/{{network_id}}/{{peering_id}}
```
