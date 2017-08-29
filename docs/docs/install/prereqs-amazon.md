# Creating security groups

Two security groups are needed. We are naming them `titusapp` and `titusmaster-mainvpc`:

<img src="../../images/secgroups.png" />

## Infrastructure security group

This is for the `titusmaster-mainvpc` security group

For inbound
- From titusmaster-mainvpc security group, ALL TCP, All ICMP
- From anywhere (including Internet), SSH

<img src="../../images/titusmaster-mainvpc-secgroup-inbound.png" />

For outbound
- All traffic

<img src="../../images/titusmaster-mainvpc-secgroup-outbound.png" />

## App security group

This is for the `titusapp` 

For inbound and outbound
- Up to your application needs

# Creating IAM Roles

Three IAM roles are needed. We are naming them `titusmasterInstanceProfile' and 'titusappwiths3InstanceProfile'
and 'titusappnos3InstanceProfile'.

<img src="../../images/iamroles.png" />

## Infrastructure IAM Role

For now, we are using a wide open IAM role. We can refine this later.

<img src="../../images/titusmasterinstanceprofilepolicy.png" />

## App IAM Roles

You need to allow this IAM Role to be assumed into via the Infrastructure Role. You can do this by setting
up trusted relationships. It should look like this:

<img src="../../images/titusappinstanceprofile-trust.png" />

The Trust relationship should look like:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    },
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ACCOUNTID:role/titusmasterInstanceProfile"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

For permissions, pick two sets of permissions that matter to your apps. We created one with S3 read access
and one without to be able to test the IAM role support feature.

