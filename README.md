# IAM-policy-restricted-Route-53-Subdomain-level-hosted-zone

## Description:-

Here we are going to give Route 53 access to an IAM user to create records under a subdomain alone.

## Pre-requisites:-

###### Main domain: anusha123.xyz
###### Sub-domain : test.anusha123.xyz

We can restrict access by hosted zone, but not by sub-domain. The Route53 hosted zones should be split up by subdomain to restrict access to specific subdomains.

### Create a hosted zone for the subdomain in Route 53

Create a hosted zone with the same name as the subdomain 'test.anusha123.xyz'

1. Open the Route 53 console.
2. In the navigation pane, choose Hosted zones.
3. Choose Create hosted zone.
4. In the right pane, enter the name of the subdomain.
5. For Type, accept the default value of Public hosted zone.
6. Choose Create hosted zone.

![1](https://user-images.githubusercontent.com/97517424/161685204-63e867ab-afb9-4fc2-b74c-0e65a007fbd7.png)

### Add NS records to route traffic to subdomain

7. Find the name servers that were assigned to the new hosted zone.
8. Select the hosted zone for the main domain.
9. Choose Create record.
10. For Name, enter the name of the subdomain.
11. For Value, enter the names of the name servers.
12. For Record type, choose NS - Name servers for a hosted zone.
13. For Route Policy, choose Simple routing.
14. Choose Create Records.

![2](https://user-images.githubusercontent.com/97517424/161685771-41492f6b-c6c5-4728-80ae-c5a201453c59.png)

## IAM policy and user

In a policy, we can grant access to the hosted zone of subdomain alone by using ARN of the subdomain hosted zone:

```html
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:GetHostedZone",
                "route53:ListResourceRecordSets",
                "route53:ListHostedZones",
                "route53:ChangeResourceRecordSets",
                "route53:ListResourceRecordSets",
                "route53:GetHostedZoneCount",
                "route53:ListHostedZonesByName"
            ],
            "Resource": "arn:aws:route53:::hostedzone/Z07261433KMZ1RWHYN3LE"
        }
    ]
}
```

Go to IAM users console -> Create user "sam" Using console access setup and attach the above policy.

![iam](https://user-images.githubusercontent.com/97517424/161686682-45b15246-1bab-4c93-836f-53786eff376b.png)

## Conclusion:-

IAM User 'sam' can now access route 53 to manage his subdomain hosted zone without accessing the main domain.

##### *sam accessing 'test.anusha123.xyz'*
![3](https://user-images.githubusercontent.com/97517424/161686907-d68b1608-23f6-4880-a97b-a764a76b738f.png)

##### *sam accessing 'anusha123.xyz'*
![5](https://user-images.githubusercontent.com/97517424/161686913-44ad462a-2464-4a5d-bbee-f94a0554cf09.png)
