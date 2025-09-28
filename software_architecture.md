flowchart TD
    subgraph Frontend["フロントエンド層"]
        UI["SPA (React/Next.js)"]
        AuthModule["認証モジュール (Cognito連携)"]
    end

    subgraph API["API層 (.NET/C# Lambda)"]
        APIRouter["APIルーター"]
        Controller["Controller層"]
        Service["Service層 (ドメインロジック)"]
        Repository["Repository層"]
    end

    subgraph Domain["ドメイン層"]
        AssetModel["資産モデル"]
        LendingModel["貸出モデル"]
        HistoryModel["履歴モデル"]
    end

    subgraph Infra["データアクセス層"]
        DynamoRepo["DynamoDB Adapter"]
        SearchRepo["OpenSearch Adapter"]
        StorageRepo["S3 Adapter"]
    end

    subgraph External["外部サービス"]
        SSOProvider["SAML / OIDC IdP"]
        Monitoring["CloudWatch API"]
    end

    UI --> AuthModule --> SSOProvider
    UI --> APIRouter
    APIRouter --> Controller --> Service --> Domain
    Service --> Repository
    Repository --> DynamoRepo
    Repository --> SearchRepo
    Repository --> StorageRepo
    Monitoring --> Service
