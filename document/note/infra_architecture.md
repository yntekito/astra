flowchart TD
    subgraph Client["クライアント"]
        Web["SPA (React/Next.js)"]
        Mobile["モバイルアプリ"]
    end

    subgraph AWS["AWS (東京リージョン)"]
        subgraph Auth["認証"]
            Cognito["Amazon Cognito (SAML / OIDC)"]
        end

        subgraph API["API層"]
            APIGW["API Gateway"]
            Lambda["Lambda (.NET/C#)"]
        end

        subgraph Data["データストア"]
            DynamoDB["DynamoDB"]
            S3["S3"]
            OpenSearch["OpenSearch Service"]
        end

        subgraph Event["イベント処理"]
            Streams["DynamoDB Streams"]
            SQS["SQS"]
            EventBridge["EventBridge"]
        end

        subgraph Monitor["監視・運用"]
            CloudWatch["CloudWatch"]
            CloudTrail["CloudTrail"]
            XRay["X-Ray"]
        end
    end

    Web -->|HTTPS| APIGW
    Mobile -->|HTTPS| APIGW
    APIGW --> Lambda
    Lambda --> DynamoDB
    Lambda --> S3
    Lambda --> OpenSearch

    DynamoDB --> Streams --> Lambda
    Lambda --> SQS --> Lambda
    Lambda --> EventBridge

    Cognito --> APIGW
    Lambda --> CloudWatch
    AWS --> CloudTrail
