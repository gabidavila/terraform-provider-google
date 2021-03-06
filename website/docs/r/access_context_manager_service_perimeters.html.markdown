---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    AUTO GENERATED CODE     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "Access Context Manager (VPC Service Controls)"
layout: "google"
page_title: "Google: google_access_context_manager_service_perimeters"
sidebar_current: "docs-google-access-context-manager-service-perimeters"
description: |-
  Replace all existing Service Perimeters in an Access Policy with the Service Perimeters provided.
---

# google\_access\_context\_manager\_service\_perimeters

Replace all existing Service Perimeters in an Access Policy with the Service Perimeters provided. This is done atomically.
This is a bulk edit of all Service Perimeters and may override existing Service Perimeters created by `google_access_context_manager_service_perimeter`,
thus causing a permadiff if used alongside `google_access_context_manager_service_perimeter` on the same parent.


To get more information about ServicePerimeters, see:

* [API documentation](https://cloud.google.com/access-context-manager/docs/reference/rest/v1/accessPolicies.servicePerimeters)
* How-to Guides
    * [Service Perimeter Quickstart](https://cloud.google.com/vpc-service-controls/docs/quickstart)

## Example Usage - Access Context Manager Service Perimeters Basic


```hcl
resource "google_access_context_manager_service_perimeters" "service-perimeter" {
  parent = "accessPolicies/${google_access_context_manager_access_policy.access-policy.name}"

  service_perimeters {
    name   = "accessPolicies/${google_access_context_manager_access_policy.access-policy.name}/servicePerimeters/"
    title  = ""
    status {
      restricted_services = ["storage.googleapis.com"]
    }
  }

  service_perimeters {
    name   = "accessPolicies/${google_access_context_manager_access_policy.access-policy.name}/servicePerimeters/"
    title  = ""
    status {
      restricted_services = ["bigtable.googleapis.com"]
    }
  }
}

resource "google_access_context_manager_access_level" "access-level" {
  parent = "accessPolicies/${google_access_context_manager_access_policy.access-policy.name}"
  name   = "accessPolicies/${google_access_context_manager_access_policy.access-policy.name}/accessLevels/chromeos_no_lock"
  title  = "chromeos_no_lock"
  basic {
    conditions {
      device_policy {
        require_screen_lock = false
        os_constraints {
          os_type = "DESKTOP_CHROME_OS"
        }
      }
      regions = [
        "CH",
        "IT",
        "US",
      ]
    }
  }
}

resource "google_access_context_manager_access_policy" "access-policy" {
  parent = "organizations/123456789"
  title  = "my policy"
}
```

## Argument Reference

The following arguments are supported:


* `parent` -
  (Required)
  The AccessPolicy this ServicePerimeter lives in.
  Format: accessPolicies/{policy_id}


- - -


* `service_perimeters` -
  (Optional)
  The desired Service Perimeters that should replace all existing Service Perimeters in the Access Policy.
  Structure is documented below.


The `service_perimeters` block supports:

* `name` -
  (Required)
  Resource name for the ServicePerimeter. The short_name component must
  begin with a letter and only include alphanumeric and '_'.
  Format: accessPolicies/{policy_id}/servicePerimeters/{short_name}

* `title` -
  (Required)
  Human readable title. Must be unique within the Policy.

* `description` -
  (Optional)
  Description of the ServicePerimeter and its use. Does not affect
  behavior.

* `create_time` -
  Time the AccessPolicy was created in UTC.

* `update_time` -
  Time the AccessPolicy was updated in UTC.

* `perimeter_type` -
  (Optional)
  Specifies the type of the Perimeter. There are two types: regular and
  bridge. Regular Service Perimeter contains resources, access levels,
  and restricted services. Every resource can be in at most
  ONE regular Service Perimeter.
  In addition to being in a regular service perimeter, a resource can also
  be in zero or more perimeter bridges. A perimeter bridge only contains
  resources. Cross project operations are permitted if all effected
  resources share some perimeter (whether bridge or regular). Perimeter
  Bridge does not contain access levels or services: those are governed
  entirely by the regular perimeter that resource is in.
  Perimeter Bridges are typically useful when building more complex
  topologies with many independent perimeters that need to share some data
  with a common perimeter, but should not be able to share data among
  themselves.
  Default value is `PERIMETER_TYPE_REGULAR`.
  Possible values are `PERIMETER_TYPE_REGULAR` and `PERIMETER_TYPE_BRIDGE`.

* `status` -
  (Optional)
  ServicePerimeter configuration. Specifies sets of resources,
  restricted services and access levels that determine
  perimeter content and boundaries.
  Structure is documented below.

* `spec` -
  (Optional)
  Proposed (or dry run) ServicePerimeter configuration.
  This configuration allows to specify and test ServicePerimeter configuration
  without enforcing actual access restrictions. Only allowed to be set when
  the `useExplicitDryRunSpec` flag is set.
  Structure is documented below.

* `use_explicit_dry_run_spec` -
  (Optional)
  Use explicit dry run spec flag. Ordinarily, a dry-run spec implicitly exists
  for all Service Perimeters, and that spec is identical to the status for those
  Service Perimeters. When this flag is set, it inhibits the generation of the
  implicit spec, thereby allowing the user to explicitly provide a
  configuration ("spec") to use in a dry-run version of the Service Perimeter.
  This allows the user to test changes to the enforced config ("status") without
  actually enforcing them. This testing is done through analyzing the differences
  between currently enforced and suggested restrictions. useExplicitDryRunSpec must
  bet set to True if any of the fields in the spec are set to non-default values.


The `status` block supports:

* `resources` -
  (Optional)
  A list of GCP resources that are inside of the service perimeter.
  Currently only projects are allowed.
  Format: projects/{project_number}

* `access_levels` -
  (Optional)
  A list of AccessLevel resource names that allow resources within
  the ServicePerimeter to be accessed from the internet.
  AccessLevels listed must be in the same policy as this
  ServicePerimeter. Referencing a nonexistent AccessLevel is a
  syntax error. If no AccessLevel names are listed, resources within
  the perimeter can only be accessed via GCP calls with request
  origins within the perimeter. For Service Perimeter Bridge, must
  be empty.
  Format: accessPolicies/{policy_id}/accessLevels/{access_level_name}

* `restricted_services` -
  (Optional)
  GCP services that are subject to the Service Perimeter
  restrictions. Must contain a list of services. For example, if
  `storage.googleapis.com` is specified, access to the storage
  buckets inside the perimeter must meet the perimeter's access
  restrictions.

* `vpc_accessible_services` -
  (Optional)
  Specifies how APIs are allowed to communicate within the Service
  Perimeter.
  Structure is documented below.


The `vpc_accessible_services` block supports:

* `enable_restriction` -
  (Optional)
  Whether to restrict API calls within the Service Perimeter to the
  list of APIs specified in 'allowedServices'.

* `allowed_services` -
  (Optional)
  The list of APIs usable within the Service Perimeter.
  Must be empty unless `enableRestriction` is True.

The `spec` block supports:

* `resources` -
  (Optional)
  A list of GCP resources that are inside of the service perimeter.
  Currently only projects are allowed.
  Format: projects/{project_number}

* `access_levels` -
  (Optional)
  A list of AccessLevel resource names that allow resources within
  the ServicePerimeter to be accessed from the internet.
  AccessLevels listed must be in the same policy as this
  ServicePerimeter. Referencing a nonexistent AccessLevel is a
  syntax error. If no AccessLevel names are listed, resources within
  the perimeter can only be accessed via GCP calls with request
  origins within the perimeter. For Service Perimeter Bridge, must
  be empty.
  Format: accessPolicies/{policy_id}/accessLevels/{access_level_name}

* `restricted_services` -
  (Optional)
  GCP services that are subject to the Service Perimeter
  restrictions. Must contain a list of services. For example, if
  `storage.googleapis.com` is specified, access to the storage
  buckets inside the perimeter must meet the perimeter's access
  restrictions.

* `vpc_accessible_services` -
  (Optional)
  Specifies how APIs are allowed to communicate within the Service
  Perimeter.
  Structure is documented below.


The `vpc_accessible_services` block supports:

* `enable_restriction` -
  (Optional)
  Whether to restrict API calls within the Service Perimeter to the
  list of APIs specified in 'allowedServices'.

* `allowed_services` -
  (Optional)
  The list of APIs usable within the Service Perimeter.
  Must be empty unless `enableRestriction` is True.

## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `{{parent}}/servicePerimeters`


## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 6 minutes.
- `update` - Default is 6 minutes.
- `delete` - Default is 6 minutes.

## Import

ServicePerimeters can be imported using any of these accepted formats:

```
$ terraform import google_access_context_manager_service_perimeters.default {{parent}}/servicePerimeters
$ terraform import google_access_context_manager_service_perimeters.default {{parent}}
```
