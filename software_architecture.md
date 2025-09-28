flowchart TD
    subgraph Frontend["フロントエンド層"]
        UI[SPA (React/Next.js)]
        AuthModule[認証モジュール<br/>(Cognito連携)]
    end

    subgraph API["API層 (.NET/C# Lambda)"]
        APIRouter[API ルーター]
        Controller[Controller 層]
        Service[Service 層<br/>(ドメインロジック)]
        Repository[Repository 層]
    end

    subgraph Domain["ドメイン層"]
        AssetModel[資産モデル]
        LendingModel[貸出モデル]
        HistoryModel[履歴モデル]
    end

    subgraph Infra["データアクセス層"]
        DynamoRepo[DynamoDB Adapter]
        SearchRepo[OpenSearch Adapter]
        StorageRepo[S3 Adapter]
    end

    subgraph External["外部サービス"]
        SSOProvider[SAML / OIDC IdP]
        Monitoring[CloudWatch API]
    end

    %% 接続
    UI --> AuthModule --> SSOProvider
    UI --> APIRouter
    APIRouter --> Controller --> Service --> Domain
    Service --> Repository
    Repository --> DynamoRepo
    Repository --> SearchRepo
    Repository --> StorageRepo
    Monitoring --> Service
