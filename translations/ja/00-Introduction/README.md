<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "2bbbcded256d46a24e3f448384a2b4a2",
  "translation_date": "2025-07-29T00:13:37+00:00",
  "source_file": "00-Introduction/README.md",
  "language_code": "ja"
}
-->
# モデルコンテキストプロトコル (MCP) 入門: スケーラブルなAIアプリケーションのための重要性

[![モデルコンテキストプロトコル入門](../../../translated_images/01.a467036d886b5fb5b9cf7b39bac0e743b6ca0a4a18a492de90061daaf0cc55f0.ja.png)](https://youtu.be/agBbdiOPLQA)

_(上の画像をクリックすると、このレッスンの動画をご覧いただけます)_

生成AIアプリケーションは、自然言語プロンプトを使用してアプリとやり取りできる点で大きな進歩を遂げています。しかし、これらのアプリに時間やリソースを投資するにつれて、機能やリソースを簡単に統合できるようにし、複数のモデルを利用できるようにし、さまざまなモデルの特性に対応できるようにする必要があります。つまり、生成AIアプリの構築は最初は簡単ですが、成長して複雑になるにつれて、アーキテクチャを定義し、アプリを一貫した方法で構築するための標準に頼る必要が出てきます。ここでMCPが登場し、整理と標準化を提供します。

---

## **🔍 モデルコンテキストプロトコル (MCP) とは？**

**モデルコンテキストプロトコル (MCP)** は、**オープンで標準化されたインターフェース**であり、大規模言語モデル (LLM) が外部ツール、API、データソースとシームレスに連携できるようにします。これにより、トレーニングデータを超えたAIモデルの機能を強化し、よりスマートでスケーラブル、かつ応答性の高いAIシステムを実現します。

---

## **🎯 AIにおける標準化の重要性**

生成AIアプリケーションが複雑化するにつれて、**スケーラビリティ、拡張性、保守性**を確保するための標準を採用することが重要です。MCPは以下のニーズに対応します：

- モデルとツールの統合を統一化
- 脆弱で一時的なカスタムソリューションを削減
- 複数のモデルが1つのエコシステム内で共存可能

---

## **📚 学習目標**

この記事を読み終えると、次のことができるようになります：

- **モデルコンテキストプロトコル (MCP)** とそのユースケースを定義する
- MCPがモデルとツール間の通信をどのように標準化するかを理解する
- MCPアーキテクチャの主要コンポーネントを特定する
- 企業や開発の文脈でのMCPの実際の応用例を探る

---

## **💡 モデルコンテキストプロトコル (MCP) がもたらす変革**

### **🔗 MCPがAIの相互作用の断片化を解決**

MCPが登場する前は、モデルとツールを統合するには以下が必要でした：

- ツールとモデルのペアごとにカスタムコードを作成
- 各ベンダーごとに非標準のAPIを使用
- アップデートによる頻繁な破損
- ツールが増えるにつれてスケーラビリティが低下

### **✅ MCP標準化の利点**

| **利点**                  | **説明**                                                                      |
|--------------------------|------------------------------------------------------------------------------|
| 相互運用性              | LLMが異なるベンダーのツールとシームレスに連携                                |
| 一貫性                  | プラットフォームやツール間での統一された動作                                  |
| 再利用性                | 一度構築したツールを複数のプロジェクトやシステムで利用可能                    |
| 開発の加速              | 標準化されたプラグアンドプレイインターフェースで開発時間を短縮                |

---

## **🧱 MCPの高レベルアーキテクチャ概要**

MCPは**クライアント-サーバーモデル**に従います。このモデルでは：

- **MCPホスト**がAIモデルを実行
- **MCPクライアント**がリクエストを開始
- **MCPサーバー**がコンテキスト、ツール、機能を提供

### **主要コンポーネント：**

- **リソース** – モデル用の静的または動的データ  
- **プロンプト** – ガイド付き生成のための事前定義されたワークフロー  
- **ツール** – 検索や計算などの実行可能な機能  
- **サンプリング** – 再帰的な相互作用を通じたエージェント的な行動  

---

## MCPサーバーの動作

MCPサーバーは以下のように動作します：

- **リクエストフロー**:  
    1. MCPクライアントがMCPホストで実行中のAIモデルにリクエストを送信します。  
    2. AIモデルが外部ツールやデータを必要とする場合を特定します。  
    3. モデルが標準化されたプロトコルを使用してMCPサーバーと通信します。  

- **MCPサーバーの機能**:  
    - ツールレジストリ: 利用可能なツールとその機能のカタログを維持  
    - 認証: ツールアクセスの権限を確認  
    - リクエストハンドラー: モデルからのツールリクエストを処理  
    - レスポンスフォーマッター: ツールの出力をモデルが理解できる形式に構造化  

- **ツールの実行**:  
    - サーバーがリクエストを適切な外部ツールにルーティング  
    - ツールが検索、計算、データベースクエリなどの専門機能を実行  
    - 結果が一貫した形式でモデルに返される  

- **レスポンスの完成**:  
    - AIモデルがツールの出力をレスポンスに組み込みます。  
    - 最終的なレスポンスがクライアントアプリケーションに送信されます。  

```mermaid
---
title: MCP Server Architecture and Component Interactions
description: A diagram showing how AI models interact with MCP servers and various tools, depicting the request flow and server components including Tool Registry, Authentication, Request Handler, and Response Formatter
---
graph TD
    A[AI Model in MCP Host] <-->|MCP Protocol| B[MCP Server]
    B <-->|Tool Interface| C[Tool 1: Web Search]
    B <-->|Tool Interface| D[Tool 2: Calculator]
    B <-->|Tool Interface| E[Tool 3: Database Access]
    B <-->|Tool Interface| F[Tool 4: File System]
    
    Client[MCP Client/Application] -->|Sends Request| A
    A -->|Returns Response| Client
    
    subgraph "MCP Server Components"
        B
        G[Tool Registry]
        H[Authentication]
        I[Request Handler]
        J[Response Formatter]
    end
    
    B <--> G
    B <--> H
    B <--> I
    B <--> J
    
    style A fill:#f9d5e5,stroke:#333,stroke-width:2px
    style B fill:#eeeeee,stroke:#333,stroke-width:2px
    style Client fill:#d5e8f9,stroke:#333,stroke-width:2px
    style C fill:#c2f0c2,stroke:#333,stroke-width:1px
    style D fill:#c2f0c2,stroke:#333,stroke-width:1px
    style E fill:#c2f0c2,stroke:#333,stroke-width:1px
    style F fill:#c2f0c2,stroke:#333,stroke-width:1px    
```

## 👨‍💻 MCPサーバーの構築方法 (例付き)

MCPサーバーを使用すると、LLMの機能をデータや機能で拡張できます。

試してみたいですか？以下は、さまざまな言語でシンプルなMCPサーバーを作成する例です：

- **Pythonの例**: https://github.com/modelcontextprotocol/python-sdk  
- **TypeScriptの例**: https://github.com/modelcontextprotocol/typescript-sdk  
- **Javaの例**: https://github.com/modelcontextprotocol/java-sdk  
- **C#/.NETの例**: https://github.com/modelcontextprotocol/csharp-sdk  

---

## 🌍 MCPの実際のユースケース

MCPはAIの能力を拡張することで、幅広いアプリケーションを可能にします：

| **アプリケーション**          | **説明**                                                                      |
|------------------------------|------------------------------------------------------------------------------|
| 企業データ統合              | LLMをデータベース、CRM、または内部ツールに接続                                |
| エージェント型AIシステム     | ツールアクセスと意思決定ワークフローを備えた自律エージェントを実現            |
| マルチモーダルアプリケーション | テキスト、画像、音声ツールを単一の統一されたAIアプリ内で組み合わせる          |
| リアルタイムデータ統合       | AIとのやり取りにライブデータを取り入れ、より正確で最新の出力を提供            |

### 🧠 MCP = AI相互作用のユニバーサルスタンダード

モデルコンテキストプロトコル (MCP) は、AI相互作用のユニバーサルスタンダードとして機能します。これは、USB-Cがデバイスの物理的接続を標準化したのと同じような役割を果たします。AIの世界では、MCPが一貫したインターフェースを提供し、モデル (クライアント) が外部ツールやデータプロバイダー (サーバー) とシームレスに統合できるようにします。これにより、各APIやデータソースごとに多様でカスタムなプロトコルを必要とする状況が解消されます。

MCPの下では、MCP互換ツール (MCPサーバーと呼ばれる) が統一された標準に従います。これらのサーバーは、提供するツールやアクションをリストし、AIエージェントからリクエストされた際にそれらを実行します。MCPをサポートするAIエージェントプラットフォームは、サーバーから利用可能なツールを発見し、この標準プロトコルを通じてそれらを呼び出すことができます。

### 💡 知識へのアクセスを促進

ツールの提供に加えて、MCPは知識へのアクセスも促進します。これにより、アプリケーションがさまざまなデータソースにリンクすることで、大規模言語モデル (LLM) にコンテキストを提供できます。たとえば、MCPサーバーが企業のドキュメントリポジトリを表し、エージェントが必要に応じて関連情報を取得できるようにすることが可能です。別のサーバーは、メール送信やレコード更新などの特定のアクションを処理することもできます。エージェントの視点から見ると、これらは単に使用可能なツールであり、一部のツールはデータ (知識コンテキスト) を返し、他のツールはアクションを実行します。MCPはこれらの両方を効率的に管理します。

エージェントがMCPサーバーに接続すると、標準フォーマットを通じてサーバーの利用可能な機能やアクセス可能なデータを自動的に学習します。この標準化により、動的なツールの利用が可能になります。たとえば、新しいMCPサーバーをエージェントのシステムに追加すると、その機能が即座に利用可能になり、エージェントの指示をさらにカスタマイズする必要がなくなります。

この統合は、サーバーがツールと知識の両方を提供し、システム間のシームレスなコラボレーションを確保するフローを示すmermaidダイアグラムと一致します。

### 👉 例: スケーラブルなエージェントソリューション

```mermaid
---
title: Scalable Agent Solution with MCP
description: A diagram illustrating how a user interacts with an LLM that connects to multiple MCP servers, with each server providing both knowledge and tools, creating a scalable AI system architecture
---
graph TD
    User -->|Prompt| LLM
    LLM -->|Response| User
    LLM -->|MCP| ServerA
    LLM -->|MCP| ServerB
    ServerA -->|Universal connector| ServerB
    ServerA --> KnowledgeA
    ServerA --> ToolsA
    ServerB --> KnowledgeB
    ServerB --> ToolsB

    subgraph Server A
        KnowledgeA[Knowledge]
        ToolsA[Tools]
    end

    subgraph Server B
        KnowledgeB[Knowledge]
        ToolsB[Tools]
    end
```

### 🔄 クライアント側LLM統合を伴う高度なMCPシナリオ

基本的なMCPアーキテクチャを超えて、クライアントとサーバーの両方にLLMが含まれる高度なシナリオでは、より洗練された相互作用が可能になります：

```mermaid
---
title: Advanced MCP Scenarios with Client-Server LLM Integration
description: A sequence diagram showing the detailed interaction flow between user, client application, client LLM, multiple MCP servers, and server LLM, illustrating tool discovery, user interaction, direct tool calling, and feature negotiation phases
---
sequenceDiagram
    autonumber
    actor User as 👤 User
    participant ClientApp as 🖥️ Client App
    participant ClientLLM as 🧠 Client LLM
    participant Server1 as 🔧 MCP Server 1
    participant Server2 as 📚 MCP Server 2
    participant ServerLLM as 🤖 Server LLM
    
    %% Discovery Phase
    rect rgb(220, 240, 255)
        Note over ClientApp, Server2: TOOL DISCOVERY PHASE
        ClientApp->>+Server1: Request available tools/resources
        Server1-->>-ClientApp: Return tool list (JSON)
        ClientApp->>+Server2: Request available tools/resources
        Server2-->>-ClientApp: Return tool list (JSON)
        Note right of ClientApp: Store combined tool<br/>catalog locally
    end
    
    %% User Interaction
    rect rgb(255, 240, 220)
        Note over User, ClientLLM: USER INTERACTION PHASE
        User->>+ClientApp: Enter natural language prompt
        ClientApp->>+ClientLLM: Forward prompt + tool catalog
        ClientLLM->>-ClientLLM: Analyze prompt & select tools
    end
    
    %% Scenario A: Direct Tool Calling
    alt Direct Tool Calling
        rect rgb(220, 255, 220)
            Note over ClientApp, Server1: SCENARIO A: DIRECT TOOL CALLING
            ClientLLM->>+ClientApp: Request tool execution
            ClientApp->>+Server1: Execute specific tool
            Server1-->>-ClientApp: Return results
            ClientApp->>+ClientLLM: Process results
            ClientLLM-->>-ClientApp: Generate response
            ClientApp-->>-User: Display final answer
        end
    
    %% Scenario B: Feature Negotiation (VS Code style)
    else Feature Negotiation (VS Code style)
        rect rgb(255, 220, 220)
            Note over ClientApp, ServerLLM: SCENARIO B: FEATURE NEGOTIATION
            ClientLLM->>+ClientApp: Identify needed capabilities
            ClientApp->>+Server2: Negotiate features/capabilities
            Server2->>+ServerLLM: Request additional context
            ServerLLM-->>-Server2: Provide context
            Server2-->>-ClientApp: Return available features
            ClientApp->>+Server2: Call negotiated tools
            Server2-->>-ClientApp: Return results
            ClientApp->>+ClientLLM: Process results
            ClientLLM-->>-ClientApp: Generate response
            ClientApp-->>-User: Display final answer
        end
    end
```

---

## 🔐 MCPの実用的な利点

MCPを使用することで得られる実用的な利点は次のとおりです：

- **最新性**: モデルがトレーニングデータを超えた最新情報にアクセス可能  
- **機能拡張**: モデルがトレーニングされていないタスクに特化したツールを活用可能  
- **幻覚の削減**: 外部データソースが事実に基づいた根拠を提供  
- **プライバシー**: 機密データをプロンプトに埋め込むのではなく、安全な環境内に保持可能  

---

## 📌 重要なポイント

MCPを使用する際の重要なポイントは次のとおりです：

- **MCP** はAIモデルがツールやデータとやり取りする方法を標準化  
- **拡張性、一貫性、相互運用性**を促進  
- MCPは**開発時間を短縮し、信頼性を向上させ、モデルの機能を拡張**  
- クライアント-サーバーアーキテクチャにより、**柔軟で拡張可能なAIアプリケーション**を実現  

---

## 🧠 演習

構築したいAIアプリケーションについて考えてみましょう。

- どのような**外部ツールやデータ**がその機能を強化できますか？  
- MCPが統合を**より簡単で信頼性の高いもの**にする方法は？  

---

## 追加リソース

- [MCP GitHubリポジトリ](https://github.com/modelcontextprotocol)

---

## 次に進む

次へ: [第1章: コアコンセプト](../01-CoreConcepts/README.md)

**免責事項**:  
この文書は、AI翻訳サービス [Co-op Translator](https://github.com/Azure/co-op-translator) を使用して翻訳されています。正確性を追求しておりますが、自動翻訳には誤りや不正確な部分が含まれる可能性があります。元の言語で記載された文書を正式な情報源としてお考えください。重要な情報については、専門の人間による翻訳を推奨します。この翻訳の使用に起因する誤解や誤解釈について、当方は一切の責任を負いません。