PDS 運用設定
==============

# 行ったこと

[公式ドキュメント](https://github.com/bluesky-social/pds/blob/main/README.md)に沿って `installer.sh` を実行

`/pds` に設定に必要なコンテナ環境が構築される。

そのまま管理者のアカウントを作成し。利用可能になる。

デフォルトはCaddyをWebプロクシとして起動し、証明書発行処理を行う。

しかしCaddyは発行されるハンドル毎に一つずつ証明書を発行し、適用するためカウント数が増えると運用が煩雑になるおそれがある。基本は自動更新で問題ないが…。

そこでnginxをウェブサーバに置き換え、証明書を Cloudflare で発行されるものを使用します。


まだデフォルト作成される pds.envファイルを 環境変数をまとめて管理するための `envs` ディレクト内に格納し管理するように変更


# ディレクトリ構造

以下の形に整理しています

```plain
├configs
│ └nginx　            -- nginx要設定ディレクトリ
│    ├conf.d
│    │  └default.conf -- nginxからPDSにプロクシする設定
│    └www
│      └/var/www 配下に相当する場所、コンテンツ置き場
├data
│  └PDS内で扱うデータ、自動生成される
│     ├accounts.sqlite  -- アカウント管理
│     ├actors           -- プロフィールで操作されたもの、アバター画像など
│     ├blocks           -- わからない
│     ├did_cache.sqlite -- ハンドルとDIDのキャッシュ
│     ├sequencer.sqlite -- 投稿のシーケンサー管理
├envs
│　├nginx.env       -- nginxの環境設定
│  └pds.env         -- PDSの環境設定
├.gitignore
├pds.env-example     -- PDS設定ファイルのサンプル設定
└README.md           -- このドキュメント
```




# デフォルトの場合
`/pds` ディレクトリ内に設定ファイルも実際のDBファイルもそのままごちゃごちゃに散らかります。