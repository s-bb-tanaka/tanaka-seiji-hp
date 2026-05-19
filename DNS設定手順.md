# tanaka-seiji.jp の DNS 設定手順(ムームードメイン)

## 現状

- ドメイン管理: **ムームードメイン**(`dns01.muumuu-domain.com` / `dns02.muumuu-domain.com`)
- Vercelプロジェクト: `tanaka-seiji-hp` (ドメイン追加済み、DNS待ち)
- 公開暫定URL: https://tanaka-seiji-hp.vercel.app

## やること

ムームードメインのコントロールパネルで、以下のDNSレコードを設定してください。

### ① ムームードメインにログイン

1. https://muumuu-domain.com/ にアクセス → ログイン
2. 左メニュー「**ムームーDNS**」をクリック
3. `tanaka-seiji.jp` の行の右側「**変更**」ボタンをクリック

### ② カスタム設定で以下のレコードを追加

「設定2」のセクションに、以下の2行を入力します。

| サブドメイン | 種別 | 内容 | 優先度 |
|---|---|---|---|
| (空欄) | **A** | `76.76.21.21` | (空欄) |
| `www` | **CNAME** | `cname.vercel-dns.com.` | (空欄) |

> ⚠️ CNAMEの末尾の `.` (ピリオド) は必須です(忘れずに)。
> 「(空欄)」は何も入力しません。

### ③ 「セットアップ情報変更」ボタンをクリックして保存

## 反映までの時間

- **5分〜数時間** で反映されます(多くの場合10〜30分)
- 反映後、Vercel が自動でSSL証明書(Let's Encrypt)を発行します
- 完了すると `https://tanaka-seiji.jp` でアクセス可能になります

## 反映確認の方法

### 方法A: ブラウザで確認

`https://tanaka-seiji.jp` にアクセスし、サイトが表示されればOK。

### 方法B: コマンドで確認

```powershell
# Aレコードが 76.76.21.21 になっているか確認
nslookup tanaka-seiji.jp
```

`Address: 76.76.21.21` のような表示があれば成功。

### 方法C: Vercel ダッシュボードで確認

1. https://vercel.com/s-bb-tanakas-projects/tanaka-seiji-hp/settings/domains
2. `tanaka-seiji.jp` の横が「**Valid Configuration**」(緑のチェック)になっているか確認

## トラブルシューティング

### 「Invalid Configuration」が消えない場合

- DNS伝播に時間がかかっています。最大24時間お待ちください。
- Vercel側で `nameservers が ns1.vercel-dns.com を指していない` 警告が出ても、Aレコード+CNAMEで運用できます(警告は無視してOK)。

### メール(info@tanaka-seiji.jp)が届かなくなった場合

メール用のMXレコードは変更していないため、影響しません。ただし、もし元々ムームーDNSで「ロリポップ!レンタルサーバー」等のMX設定がある場合は、念のため設定変更前にスクリーンショットを取っておくと安心です。

## 完了後

DNS反映が確認できたら、以下も行うとSEO効果が上がります。

1. **Google Search Console** に `https://tanaka-seiji.jp` を登録
   - https://search.google.com/search-console
   - サイトマップURL: `https://tanaka-seiji.jp/sitemap.xml` を送信
2. **Google Analytics 4** を導入(任意)
3. **www → 非www へのリダイレクト** が自動で機能していることを確認
   - `https://www.tanaka-seiji.jp` → `https://tanaka-seiji.jp` に遷移すればOK
