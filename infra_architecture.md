flowchart TD
    subgraph Client["📱 クライアント"]
        Web[SPA (React/Next.js)]
        Mobile[モバイルアプリ]
    end

    subgraph AWS["☁️ AWS (東京リージョン)"]
        subgraph Auth["認証"]
            Cognito[Amazon Cognito<br/>SAML / OIDC SSO]
        end

        subgraph API["API層"]
            APIGW[API Gateway<br/>(HTTP API)]
            Lambda[Lambda (.NET/C#)]
        end

        subgraph Data["データストア"]
            DynamoDB[(DynamoDB<br/>資産・貸出情報)]
            S3[(S3<br/>添付ファイル・バックアップ)]
            OpenSearch[(OpenSearch Service<br/>検索用インデックス)]
        end

        subgraph Event["イベント処理"]
            Streams[DynamoDB Streams]
            SQS[SQS]
            EventBridge[EventBridge]
        end

        subgraph Monitor["監視・運用"]
            CloudWatch[CloudWatch Logs/Metrics]
            CloudTrail[CloudTrail]
            XRay[X-Ray]
        end
    end

    %% フロー
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
    CloudWatch --> Lambda
    CloudTrail --> AWS
