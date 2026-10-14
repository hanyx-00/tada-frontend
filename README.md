<div align="center">

# 📔 TADA ｜ AIダイアリー — Frontend

### 一日を書くと、AI が絵にして残してくれる

**人物キュレーター（Curator）画面 担当 ｜ 朱 韓榮（JU HAN YOUNG）**

![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React_19-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-FF4154?style=for-the-badge&logo=reactquery&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS_4-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)

[**▶ Demo**](https://tada-frontend-seven.vercel.app) ・ [**Backend（設計の詳細）**](https://github.com/hanyx-00/tada-was) ・ [**Curator のコード**](./domains/curator/) ・ [한국어](./README.ko.md)

</div>

---

## 🧭 このページについて

TADA は **5名のチームで開発した AI 絵日記サービス** です（2026.08 – 09）。<br>
このリポジトリはフロントエンド（Next.js）を私がフォークしたものです。

私は **人物キュレーター** を担当し、バックエンドの API と、このリポジトリの `domains/curator` 画面を両方実装しました。<br>
人物判定・トランザクション・同時実行制御など **設計の中心はバックエンドの README** で説明しています 👉 [hanyx-00/tada-was](https://github.com/hanyx-00/tada-was)

---

## 🖼 担当した画面

| 画面 | 内容 |
| --- | --- |
| **人物一覧パネル** | 日記に出てきた人物をカードで表示。名前で絞り込み検索 |
| **人物詳細モーダル** | 「記録」と「思い出」の2タブ構成 |
| ├ 記録タブ | その人が登場する日記のタイムライン。新しい順 / 古い順、無限スクロール |
| └ 思い出タブ | その人と一緒の日記・ステッカーをまとめて表示 |
| **人物の訂正モーダル** | AI の判定が違うとき「既存の人物を選ぶ」か「新しい名前で作る」を選んで付け替え |
| **名前の変更** | 詳細モーダルから人物の表示名を変更 |
| **思い出の振り返りパネル** | 過去の日記を1件提案。「別の思い出」で再提案 |

---

## ⚙️ 実装のポイント

**サーバー状態は TanStack Query に集約**
- 一覧・詳細・思い出は `useQuery`、タイムラインは `useInfiniteQuery` ＋ `IntersectionObserver` で次のページを自動で読み込みます。
- クエリキーを `["persons", personId, "detail" | "timeline" | "memories"]` の階層にそろえ、訂正・名前変更が成功したら `["persons"]` を無効化するだけで、一覧・詳細・タイムライン・思い出がまとめて最新化されるようにしました。
- 振り返りパネルは `keepPreviousData` を使い、「別の思い出」を押しても前のカードを表示したまま次を読み込みます（画面のちらつき防止）。

**ドメイン単位の構成**
- `api/`（API 呼び出し）・`hooks/`（Query）・`components/`（画面）・`types/`（型）に分け、`app/page.tsx` は各ドメインの部品を組み立てるだけにしています。
- バックエンドの DTO（自分で設計したもの）に合わせた型を `types/curator.ts` に定義し、API と画面の間でデータの形がずれないようにしています。

**状態ごとの表示**
- 読み込み中はスケルトン、空のときと失敗したときは別々のメッセージを表示します。

---

## 📁 構成

```text
tada-frontend/
├── app/                  # ルーティング（layout, page, providers）
├── domains/
│   ├── auth/             # ログイン・会員登録
│   ├── calendar/         # メインカレンダー
│   ├── diary/            # 日記の作成・詳細・ゴミ箱
│   ├── sticker/          # ステッカーアルバム
│   ├── search/           # AI 日記検索
│   ├── curator/          # 👈 人物・思い出（担当）
│   │   ├── api/          #    curatorApi.ts
│   │   ├── hooks/        #    usePersons / usePersonTimeline / useCorrectPerson …
│   │   ├── components/   #    PeoplePanel / PersonDetailModal / PersonCorrectionModal …
│   │   └── types/        #    curator.ts
│   └── guest/            # 未ログイン向けランディング
└── shared/               # 共通 UI・API クライアント・トークン管理
```

---

## 🚀 実行方法

```bash
npm install
npm run dev        # http://localhost:3000
```

<sub>環境変数・スクリプト・各ドメインの詳細は [한국어 README](./README.ko.md) をご覧ください。</sub>

---

## 👥 チーム

5名・ドメイン別に分担<br>
**curator（人物キュレーター）** — 한영（私）

---

<div align="center">

**朱 韓榮（JU HAN YOUNG）** ・ バックエンドエンジニア志望<br>
📫 a98220919@gmail.com ・ 🐙 [github.com/hanyx-00](https://github.com/hanyx-00)

</div>
