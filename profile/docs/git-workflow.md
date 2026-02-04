# Git 開発ガイドライン

## 重要ルール

事故（意図しないコードのリリース）を防ぐため、以下のルールを**遵守**してください。

1. **始点は必ず `main`**
   作業開始時は必ず本番環境（`main`）からブランチを切ってください。
2. **ブランチを汚さない（逆マージ禁止）**
   `develop` や他人の変更を自分の作業ブランチにマージしないでください。

---

## 1. 基本的なブランチ構成

### メインブランチ

- `main` - 本番環境用
- `develop` - ステージング環境用

### 機能ブランチ

**必ず `main` ブランチから派生させて作成：**

- `feature/` - 新機能開発用（例：`feature/login-page`）
- `fix/` - 修正用（例：`fix/header-layout`）
- `update/` - 更新用（例：`update/news`）

---

## 2. コミットメッセージ

推奨するプレフィックス：

- `[add]` - 機能・ファイルの追加
- `[fix]` - 修正対応
- `[update]` - 更新対応
- `[remove]` - 削除対応

---

## 3. ワークフロー詳細

### Step 1: ブランチ作成

必ず `main` から作成します。

```bash
git checkout main
git pull origin main
git checkout -b feature/topic-name
```

### Step 2: テスト環境への反映

テスト時は `develop` に向かってマージします。

```bash
git checkout develop
git merge --no-ff feature/topic-name
git push origin develop
```

※ コンフリクトした場合は、`develop` 上で解消してください（自分のブランチは汚さない）。

### Step 3: 本番環境への反映

リリース時は `main` に向かってマージします。

```bash
git checkout main
git merge --no-ff feature/topic-name
git push origin main
```

### Step 4: ブランチの削除

本番環境への反映が完了し、不要になった作業ブランチは削除します。

```bash
# ローカルブランチの削除
git branch -d feature/topic-name

# リモートブランチの削除（必要な場合）
git push origin --delete feature/topic-name
```

---

## 4. なぜこのルールが必要なのか？

**【発生しうる事故のケース】**

1. Aさんが `feature/A` を `develop` にマージ（検証中・リリース不可）
2. Bさんが `develop` を自分の `feature/B` にマージ（最新化のつもり）
    - **ここで Bさんのブランチに Aさんのコードが混入！**
3. Bさんが `feature/B` を `main` にリリース
    - **Aさんの未完成コードも一緒に本番公開されてしまう**

これを防ぐため、作業ブランチは最後まで「自分だけの純粋な状態」を保つ必要があります。

---

## 5. 注意事項

- **マージは「上書き保存」ではなく、異なる変更履歴を一つにまとめる「統合」です**
- **プルリクエストについて**
  本ガイドラインを理解・遵守している限り、プルリクエストによる承認は不要です。各々の責任でマージを行ってください。
- **作業が終わったらブランチを削除する**
  `main` への反映が完了した作業ブランチは、速やかに削除してください。
