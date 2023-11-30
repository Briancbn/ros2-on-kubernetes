# Setup AWS tools

Install AWS cli

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Install eksctl 

```bash
# for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"
tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz

sudo mv /tmp/eksctl /usr/local/bin
```

Reference

- <https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html>
- <https://eksctl.io/installation/#for-unix>

Create an IAM account with the following permission policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "iam:CreateInstanceProfile",
                "iam:GetPolicyVersion",
                "logs:*",
                "iam:DeletePolicy",
                "iam:CreateRole",
                "iam:AttachRolePolicy",
                "iam:PutRolePolicy",
                "autoscaling:*",
                "iam:AddRoleToInstanceProfile",
                "iam:ListInstanceProfilesForRole",
                "iam:PassRole",
                "iam:DetachRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:ListAttachedRolePolicies",
                "route53:ListResourceRecordSets",
                "kms:DescribeKey",
                "kms:CreateKey",
                "acm:ListCertificates",
                "iam:ListRolePolicies",
                "iam:DeleteOpenIDConnectProvider",
                "iam:GetRole",
                "cloudformation:ListStacks",
                "iam:GetPolicy",
                "route53:ListHostedZones",
                "iam:ListRoles",
                "iam:DeleteRole",
                "elasticloadbalancing:*",
                "iam:CreateOpenIDConnectProvider",
                "iam:CreatePolicy",
                "iam:CreateServiceLinkedRole",
                "iam:ListPolicyVersions",
                "cloudwatch:*",
                "kms:ListAliases",
                "kms:CreateAlias",
                "ec2:*",
                "iam:GetOpenIDConnectProvider",
                "eks:*",
                "iam:TagOpenIDConnectProvider",
                "iam:GetRolePolicy",
                "kms:DeleteAlias",
                "cloudformation:*",
                "elasticfilesystem:CreateMountTarget"
            ],
            "Resource": "*"
        },
    ]
}
```

After creating the user, note down the AWS `ACCESS_KEY_ID` and `SECRET_ACCESS_KEY`. Set up these environment variable to use the IAM user for AWS cli

```bash
export AWS_ACCESS_KEY_ID=<access-key-id>
export AWS_SECRET_ACCESS_KEY=<secret-access-key>
export AWS_DEFAULT_REGION=<region-code>
```

