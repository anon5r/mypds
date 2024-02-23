PDS 運用設定
==============

# 行ったこと

[公式ドキュメント](https://github.com/bluesky-social/pds/blob/main/README.md)に沿って `installer.sh` を実行

`/pds` に設定に必要なコンテナ環境が構築される。

そのまま管理者のアカウントを作成し。利用可能になる。

デフォルトはCaddyをWebプロキシとして起動し、証明書発行処理を行う。

しかしCaddyは発行されるハンドル毎に（正確にはアクセスがあった際に証明書が存在していないホスト名に対して）一つずつ証明書を発行し、適用するためカウント数が増えると運用が煩雑になるおそれがある。（基本はそれでも問題ないと思う）

そこでnginxをウェブサーバに置き換え、証明書を Cloudflare で発行されるものを使用します。


まだデフォルト作成される pds.envファイルを 環境変数をまとめて管理するための `envs` ディレクト内に格納し管理するように変更


> `pdsadmin` コマンドからは `/pds/pds.env` を直に参照されているため移動すると参照不可でエラーになる
> pds.envをenvs/に移動させないか、以下の様にリンクを貼るだけでも解決可能
```shell
ln -s envs/pds.env /pds/pds.env
```


# ディレクトリ構造

以下の形に整理しています

```plain
├configs
│ └nginx                -- nginx用設定ディレクトリ
│    ├conf.d
│    │  └default.conf   -- nginxからPDSにプロクシする設定
│    └www
│      └/var/www 配下に相当する場所、コンテンツ置き場
├data
│  └PDS内で扱うデータ、自動生成される
│     ├accounts.sqlite  -- PDSのアカウント管理
│     ├actors           -- プロフィールで操作されたもの、アバター画像など
│     ├blocks           -- わからない
│     ├did_cache.sqlite -- ハンドルとDIDのキャッシュ
│     ├sequencer.sqlite -- シーケンスID管理
├envs                   -- 環境変数置き場
│ ├nginx.env            -- nginxの環境設定
│ └pds.env              -- PDSの環境設定
├.gitignore
├pds.env-example        -- PDS設定ファイルのサンプル設定
└README.md              -- このドキュメント
```




# デフォルトの場合
`/pds` ディレクトリ内に設定ファイルも実際のDBファイルもそのままごちゃごちゃに散らかります。