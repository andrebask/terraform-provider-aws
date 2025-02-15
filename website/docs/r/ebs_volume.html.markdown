---
subcategory: "EBS (EC2)"
layout: "aws"
page_title: "AWS: aws_ebs_volume"
description: |-
  Provides an elastic block storage resource.
---

# Resource: aws_ebs_volume

Manages a single EBS volume.

## Example Usage

```terraform
resource "aws_ebs_volume" "example" {
  availability_zone = "us-west-2a"
  size              = 40

  tags = {
    Name = "HelloWorld"
  }
}
```

~> **NOTE**: At least one of `size` or `snapshot_id` is required when specifying an EBS volume

## Argument Reference

The following arguments are supported:

* `availability_zone` - (Required) The AZ where the EBS volume will exist.
* `encrypted` - (Optional) If true, the disk will be encrypted.
* `final_snapshot` - (Optional) If true, snapshot will be created before volume deletion. Any tags on the volume will be migrated to the snapshot. By default set to false
* `iops` - (Optional) The amount of IOPS to provision for the disk. Only valid for `type` of `io1`, `io2` or `gp3`.
* `multi_attach_enabled` - (Optional) Specifies whether to enable Amazon EBS Multi-Attach. Multi-Attach is supported on `io1` and `io2` volumes.
* `size` - (Optional) The size of the drive in GiBs.
* `snapshot_id` (Optional) A snapshot to base the EBS volume off of.
* `outpost_arn` - (Optional) The Amazon Resource Name (ARN) of the Outpost.
* `type` - (Optional) The type of EBS volume. Can be `standard`, `gp2`, `gp3`, `io1`, `io2`, `sc1` or `st1` (Default: `gp2`).
* `kms_key_id` - (Optional) The ARN for the KMS encryption key. When specifying `kms_key_id`, `encrypted` needs to be set to true. Note: Terraform must be running with credentials which have the `GenerateDataKeyWithoutPlaintext` permission on the specified KMS key as required by the [EBS KMS CMK volume provisioning process](https://docs.aws.amazon.com/kms/latest/developerguide/services-ebs.html#ebs-cmk) to prevent a volume from being created and almost immediately deleted.
* `tags` - (Optional) A map of tags to assign to the resource. If configured with a provider [`default_tags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.
* `throughput` - (Optional) The throughput that the volume supports, in MiB/s. Only valid for `type` of `gp3`.

~> **NOTE**: When changing the `size`, `iops` or `type` of an instance, there are [considerations](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/considerations.html) to be aware of.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The volume ID (e.g., vol-59fcb34e).
* `arn` - The volume ARN (e.g., arn:aws:ec2:us-east-1:0123456789012:volume/vol-59fcb34e).
* `tags_all` - A map of tags assigned to the resource, including those inherited from the provider [`default_tags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block).

## Timeouts

`aws_ebs_volume` provides the following [Timeouts](https://www.terraform.io/docs/configuration/blocks/resources/syntax.html#operation-timeouts) configuration options:

- `create` - (Default `5 minutes`) Used for creating volumes. This includes the time required for the volume to become available
- `update` - (Default `5 minutes`) Used for `size`, `type`, or `iops` volume changes
- `delete` - (Default `5 minutes`) Used for destroying volumes

## Import

EBS Volumes can be imported using the `id`, e.g.,

```
$ terraform import aws_ebs_volume.id vol-049df61146c4d7901
```
