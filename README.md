# 概要

当リポジトリは [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) での開発を前提としています。  
以降の説明は上記の拡張機能がインストールされていることを前提としています。  
また Nuxt に由来するファイルは以下のコマンドでインストールを行った時点から変更はありません。
```sh
npx nuxi init . --force
```

## Dev Containers での開発方法

1. `.env.example` をコピーし `.env` ファイルを作成します。
    - `cp .env.example .env`
2. リポジトリのルートディレクトリを VSCode で開き、 `Shift + Ctrl + P` を入力します。
3. `reopen in container` を入力し、表示されるコマンドを選択します。  
    ※初回起動時は何を選んでも問題ありません。  
    ![image](https://github.com/takayuki-miyazaki-adglobe-co-jp/nuxt-on-docker-template/assets/105618751/72b04717-0988-479b-9e94-e5cecc7f6964)
4. VSCode がコンテナ内で起動します。  
    ウィンドウ左下が以下の表示になっていれば完了です。  
    ![image](https://github.com/takayuki-miyazaki-adglobe-co-jp/nuxt-on-docker-template/assets/105618751/44f4c3c8-9cb5-4c3a-97a0-637568aa4fd5)
5. コンテナ起動と同時に Nuxt が起動するように設定しているため http://localhost:3000 に接続できます。

> **Note**
>
> 以下の通知が表示されている場合は `コンテナーで再度開く` を選択していただいて構いません。  
> その場合 2, 3 の手順は不要です。  
> ![image](https://github.com/takayuki-miyazaki-adglobe-co-jp/nuxt-on-docker-template/assets/105618751/bca3f4bd-2525-464b-840a-51c5d44efda7)

## コンテナ内での Git 操作

以下を実施することで `Dev Containers` で起動した VSCode のターミナルから Git の操作を行うことができます。

1. GitHub に SSH キーを登録します。
    - [登録方法](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#%E6%96%B0%E3%81%97%E3%81%84-ssh-%E3%82%AD%E3%83%BC%E3%82%92%E7%94%9F%E6%88%90%E3%81%99%E3%82%8B)
2. (Windowsのみ) ssh-agent をバックグラウンド起動するように設定します。
    - [設定方法](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows)
3. ssh-agent に秘密鍵を登録します。
    - [登録方法](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#ssh-%E3%82%AD%E3%83%BC%E3%82%92-ssh-agent-%E3%81%AB%E8%BF%BD%E5%8A%A0%E3%81%99%E3%82%8B)

## ビルド方法

`.env` のキー `NODE_ENV` の値を `production` に変更し、**コンテナの外**で以下のコマンドを入力します。
```sh
docker compose down --volumes --rmi all
docker compose up -d
```
開発用コンテナと同様、コンテナ起動と同時に Nuxt が起動するように設定しているため http://localhost:3000 に接続できます。

> **Note**
>
> `Dev Containers` からコンテナの外に出るには `Shift + Ctrl + P` を入力し、ホストプロセスによって `reopen folder locally` または `reopen folder in wsl` のいずれかを選択してください。
