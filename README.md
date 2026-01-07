# ads-data-management-app
業務データ（Impression / Click / Cost / CV）を管理・集計するWebアプリケーション。 Next.js + TypeScript + MySQL + AWS を用いた実務想定の個人開発。

## 技術スタック

### フロントエンド
- Next.js (App Router)
- React
- TypeScript

### バックエンド
- Next.js API Routes (Node.js)
- Prisma

### データベース
- MySQL (AWS RDS)

### インフラ
- AWS
  - EC2
  - RDS (MySQL)
  - S3
  - IAM
  - CloudWatch

### 認証
- NextAuth

## サービス概要
本サービスは、広告・業務データ（Impression / Click / Cost / CV）を
案件単位で登録・管理し、期間別・OS別などの条件で集計・可視化できるWebアプリケーションです。
日々のレポート作成や集計作業を効率化し、データ管理の属人化を防ぐことを目的としています。

## 解決したい課題
業務において、広告データをスプレッドシートやGASで管理しているが、
案件数やデータ量が増えるにつれて以下の課題があった。

- スプレッドシートで管理が複雑化し、運用が破綻しやすい
- データの集計に時間がかかる（毎回同じ集計を繰り返す）
- 期間別・OS別の集計が煩雑でミスが起きやすい
- 過去データの参照・再利用がしづらい
- 複数人での管理が難しく、属人化しやすい

## 想定ユーザー
- 広告運用・データ管理を行う事務担当者
- 少人数チームで業務データを管理しているWebサービス運営者
- 手作業やスプレッドシートでの集計に課題を感じている人

## 主な機能一覧
### MVP（優先して実装）
- ユーザー登録 / ログイン機能
- 案件（case）管理機能
- 業務データ登録（Impression / Click / Cost / CV）
- 期間指定によるデータ集計
- OS（iOS / Android）別集計
- 集計結果のレポート表示

### 追加機能（MVP後）
- CSVファイルによるデータ一括登録
- 管理者向け管理画面
  
## 画面構成

### 認証関連
- /login  
  - ログイン画面
- /signup  
  - 新規ユーザー登録画面

### メイン機能
- /dashboard  
  - 全案件のサマリー表示
  - 期間指定・簡易集計

- /cases  
  - 案件一覧画面

- /cases/new  
  - 案件作成画面

- /cases/[id]  
  - 案件詳細画面
  - 基本情報の確認・編集

- /cases/[id]/data  
  - 業務データ一覧（Impression / Click / Cost / CV）
  - データ登録・編集

- /reports  
  - 集計レポート画面
  - 期間指定・OS（iOS / Android）別集計

### 管理者機能（追加機能）
- /admin  
  - 管理者向けダッシュボード
  - ユーザー・案件管理

## ER図

### users
- id (PK)
- name
- email (Unique)
- passwordHash（※NextAuth運用なら不要になる場合あり）
- role（"USER" | "ADMIN"）
- createdAt
- updatedAt

### cases
- id (PK)
- name
- description（任意）
- createdByUserId (FK -> users.id)
- createdAt
- updatedAt

### case_members
- id (PK)
- caseId (FK -> cases.id)
- userId (FK -> users.id)
- role（"OWNER" | "EDITOR" | "VIEWER"）
- createdAt
- Unique(caseId, userId)

### metrics
- id (PK)
- caseId (FK -> cases.id)
- date（YYYY-MM-DD）
- os（"IOS" | "ANDROID" | "ALL"）
- impressions（int）
- clicks（int）
- cost（decimal）
- cv（int）
- createdAt
- updatedAt
- Unique(caseId, date, os)

## API一覧
## AWS構成図


