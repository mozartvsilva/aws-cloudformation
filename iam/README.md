## IAM

### AWS::IAM::ManagedPolicy

IAM Managed Policy.

```yaml
CustomManagedPolicy:
  Type: AWS::IAM::ManagedPolicy
  Properties:
    ManagedPolicyName: CustomManagedPolicyName
    PolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Sid: 'FirstStatement'
          Action:
            - "s3:PutObject"
            - "s3:GetObject"
          Effect: "Allow"
          Resource: "arn:aws:s3:::*/*"
```

### AWS::IAM::Policy

IAM Policy with specified role.

```yaml
CustomExternalPolicyWithSpecifiedRole:
  Type: AWS::IAM::Policy
  Properties:
    PolicyName: CustomExternalPolicyNameWithSpecifiedRole
    PolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Sid: 'VisualEditor0'
          Action:
            - "s3:PutObject"
            - "s3:GetObject"
          Effect: "Allow"
          Resource: "arn:aws:s3:::*/*"
    Roles:
      - !Ref CustomRole
```

IAM Policy with specified group.

```yaml
CustomExternalPolicyWithSpecifiedGroup:
  Type: AWS::IAM::Policy
  Properties:
    PolicyName: CustomExternalPolicyNameWithSpecifiedGroup
    PolicyDocument:
      Version: "2012-10-17"
      Statement:
        - Sid: 'FirstStatement'
          Action:
            - "s3:PutObject"
            - "s3:GetObject"
          Effect: "Allow"
          Resource: "arn:aws:s3:::*/*"
    Groups:
      - !Ref CustomGroup
```

### AWS::IAM::Role

IAM Role with Inline Policy, Managed Policies and AWS Managed Policies.

```yaml
CustomRole:
  Type: AWS::IAM::Role
  Properties:
    RoleName: CustomRoleName
    AssumeRolePolicyDocument:
      Version: '2012-10-17'
      Statement:
        -
          Effect: "Allow"
          Principal:
            Service:
              - "lambda.amazonaws.com"
          Action:
            - "sts:AssumeRole"
    ManagedPolicyArns:
      - !Ref CustomManagedPolicy
      - arn:aws:iam::aws:policy/AmazonRDSReadOnlyAccess
      - arn:aws:iam::aws:policy/service-role/AWSLambdaVPCAccessExecutionRole
```

IAM Role with an Embedded policy in the Policies property of the AWS::IAM::Role.

```yaml
CustomRoleWithEmbeddedPolicy:
  Type: AWS::IAM::Role
  Properties:
    RoleName: CustomRoleNameWithEmbeddedPolicy
    AssumeRolePolicyDocument:
      Version: '2012-10-17'
      Statement:
        -
          Effect: "Allow"
          Principal:
            Service:
              - "lambda.amazonaws.com"
          Action:
            - "sts:AssumeRole"
    Policies:
      - PolicyName: CustomEmbeddedPolicyName
        PolicyDocument:
          Version: '2012-10-17'
          Statement:
            - Action:
                - "logs:CreateLogStream"
                - "logs:CreateLogGroup"
                - "logs:PutLogEvents"
                - "sns:Publish"
              Effect: Allow
              Resource: "*"
            - Action:
                - "dynamodb:PutItem"
                - "dynamodb:DeleteItem"
                - "dynamodb:GetItem"
                - "dynamodb:Scan"
                - "dynamodb:Query"
                - "dynamodb:UpdateItem"
                - "dynamodb:BatchWriteItem"
              Effect: Allow
              Resource: "*"
```

### AWS::IAM::Group

IAM Group with Inline Policy and default AWS Managed Policies.

```yaml
CustomGroup:
  Type: AWS::IAM::Group
  Properties:
    GroupName: CustomGroupName
    ManagedPolicyArns:
      - arn:aws:iam::aws:policy/AmazonRDSReadOnlyAccess
      - arn:aws:iam::aws:policy/service-role/AWSLambdaVPCAccessExecutionRole
```

IAM Group with an Embedded inline policy in the Policies property of the AWS::IAM::Group.

```yaml
CustomGroupWithEmbeddedPolicy:
  Type: AWS::IAM::Group
  Properties:
    GroupName: CustomGroupNameWithEmbeddedPolicy
    ManagedPolicyArns:
      - arn:aws:iam::aws:policy/AmazonRDSReadOnlyAccess
      - arn:aws:iam::aws:policy/service-role/AWSLambdaVPCAccessExecutionRole
    Policies:
      - PolicyName: CustomGroupEmbeddedPolicyName
        PolicyDocument:
          Version: '2012-10-17'
          Statement:
            - Action:
                - "logs:CreateLogStream"
                - "logs:CreateLogGroup"
                - "logs:PutLogEvents"
                - "sns:Publish"
              Effect: Allow
                Resource: "*"
```