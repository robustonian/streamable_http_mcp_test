# MCP Server

Streamable HTTP transportを使用したModel Context Protocol (MCP) サーバーの実装です。

## 参考資料

このプロジェクトは以下の記事を参考に作成されました：
- [MCP サーバーの Streamable HTTP transport を試してみる](https://azukiazusa.dev/blog/mcp-server-streamable-http-transport/)

## 概要

このプロジェクトは、HTTP経由でMCPプロトコルを使用してAIアシスタントとやり取りできるサーバーを提供します。現在、サイコロを振る機能を持つシンプルなツールが実装されています。

## 機能

- **dice ツール**: 指定した面数のサイコロを振って結果を返します
- **Streamable HTTP Transport**: HTTPリクエスト経由でのMCP通信
- **環境変数対応**: ポート番号を環境変数で設定可能
- **エラーハンドリング**: JSON-RPC 2.0仕様に準拠したエラー処理

## セットアップ

### 必要条件

- Node.js (推奨: v18以上)
- pnpm

### インストール

```bash
pnpm install
```

### 環境設定

`.env.example`を`.env`にコピーして、必要に応じて設定を変更してください：

```bash
cp .env.example .env
```

デフォルトのポート番号は3000です。

## 使用方法

### サーバーの起動

```bash
npx tsx src/index.ts
```

または

```bash
./serve.sh
```

サーバーは `http://localhost:3000/mcp` で起動します（PORTを変更した場合は該当ポート）。

### 環境変数でのポート指定

```bash
PORT=8080 npx tsx src/index.ts
```

## API エンドポイント

- `POST /mcp`: MCPリクエストを処理するメインエンドポイント
- `GET /mcp`: 405 Method Not Allowed を返す（SSE互換性のため）
- `DELETE /mcp`: 405 Method Not Allowed を返す（ステートフルサーバー互換性のため）

## 実装されているツール

### dice ツール

サイコロを振って結果を返します。

**パラメータ:**
- `sides` (number, optional): サイコロの面数（デフォルト: 6）

**使用例:**
```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "dice",
    "arguments": {
      "sides": 20
    }
  },
  "id": 1
}
```

## 技術仕様

- **フレームワーク**: Express.js
- **言語**: TypeScript
- **MCPプロトコル**: @modelcontextprotocol/sdk
- **トランスポート**: StreamableHTTPServerTransport（ステートレス）
- **バリデーション**: Zod

## 開発

このプロジェクトはTypeScriptで書かれており、`tsx`を使用して直接実行されます。

## ライセンス

ISC