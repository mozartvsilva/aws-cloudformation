
## Api Gateway

### AWS::ApiGateway::RestApi

```yaml
ApiGatewayRestApi:
  Type: AWS::ApiGateway::RestApi
  Properties:
    Description: Api Gateway Rest API.
    FailOnWarnings: false
    Name: ApiGatewayRestApiExample
```

Configuring an EndpointConfiguration property type that specifies the endpoint types of a REST API. Valid values include:

* *EDGE*: For an edge-optimized API and its custom domain name.
* *REGIONAL*: For a regional API and its custom domain name.
* *PRIVATE*: For a private API.

```yaml
ApiGatewayRestApi:
  EndpointConfiguration:
    Types:
      - PRIVATE
```

Adding a Resource Policy allowing access using a VPC, VPCE or IP Address.

```yaml
ApiGatewayRestApi:
  Policy:
    Version: '2012-10-17'
    Statement:
    - Action:
      - "execute-api:Invoke"
      Effect: Allow
      Principal: "*"
      Resource:
        - execute-api:/*/*/*
      Condition:
        StringEquals:
          aws:SourceVpc: vpc-abcdefg
```

### AWS::ApiGateway::Method

Defining a HTTP GET  operation method.

```yaml
ApiGatewayMethod:
  Type: AWS::ApiGateway::Method
  Properties:
    RestApiId: !Ref ApiGatewayRestApi
    ApiKeyRequired: false
    AuthorizationType: NONE
    HttpMethod: GET
    MethodResponses:
      - StatusCode: 200
    ResourceId: !GetAtt [ ApiGatewayRestApi, RootResourceId ]
```

Integrating the defined method above to an AWS bucket service and returning a file content.

```yaml
ApiGatewayMethod:
  Properties:
    Integration:
      Type: AWS
      Credentials: !GetAtt [ ApiGatewayRole, Arn ]
      IntegrationHttpMethod: GET
      IntegrationResponses:
        - StatusCode: 200
      PassthroughBehavior: WHEN_NO_MATCH
      Uri: arn:aws:apigateway:us-east-1:s3:path/bucke-name/data.json
```
*ApiGatewayRole* is a resource defining roles allowing Api Gateway to access the Bucket.

### AWS::ApiGateway::Deployment

```yaml
ApiGatewayDeployment:
  Type: AWS::ApiGateway::Deployment
  DependsOn: ApiGatewayMethod
  Properties:
    RestApiId: !Ref ApiGatewayRestApi
    StageName: definedStageName
```