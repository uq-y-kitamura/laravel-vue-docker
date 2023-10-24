## 環境
- PHP8.1
- nginx1.23.1
- MySQL8.0.23
- Laravel10
- Vue3

## 手順
- プロジェクトディレクトリ直下にsrcディレクトリを作成(larevelをsrc内にインストールするため)

- イメージをビルド
  ```cmd
  y.kitamura@UQO370 laravel-vue-docker % docker compose build
  ```

- コンテナの立ち上げ
  ```cmd
  y.kitamura@UQO370 laravel-vue-docker % docker compose up -d
  ```

- コンテナに入る
  ```cmd
  y.kitamura@UQO370 laravel-vue-docker % docker compose exec app bash
  ```

- コンテナ内でLarave10をインストール
  ```cmd
    root@5b403243e9e9:/var/www/html# composer create-project --prefer-dist "laravel/laravel=10.*" .
  ```

- Larave10がインストールされていることを確認
  ```cmd
  root@5b403243e9e9:/var/www/html# php artisan -V
  
  Deprecated: PHP Startup: Use of mbstring.internal_encoding is deprecated in Unknown on line 0
  Laravel Framework 10.28.0
  ```

- http://localhost:38080/ でLaravelの初期画面が表示されるか確認
  <img width="1594" alt="スクリーンショット 2023-10-24 9 50 42" src="https://github.com/uq-y-kitamura/laravel-vue-docker/assets/110364222/fb76ff01-6b2d-480d-96c1-48bc9a7b1c79">

- マイグレーションを実行(php artisan migrate)して、appコンテナとdbコンテナの接続を確認
  ```cmd
  root@5b403243e9e9:/var/www/html# php artisan migrate

  Deprecated: PHP Startup: Use of mbstring.internal_encoding is deprecated in Unknown on line 0
  
     INFO  Preparing database.  
  
    Creating migration table ............................................................................................................... 29ms DONE
  
     INFO  Running migrations.  
  
    2014_10_12_000000_create_users_table ................................................................................................... 56ms DONE
    2014_10_12_100000_create_password_reset_tokens_table ................................................................................... 44ms DONE
    2019_08_19_000000_create_failed_jobs_table ............................................................................................ 149ms DONE
    2019_12_14_000001_create_personal_access_tokens_table .................................................................................. 51ms DONE
  ```

- パッケージをインストール
  ```cmd
  root@5b403243e9e9:/var/www/html# npm install
  ```
- Vueを動かすのに必要なパッケージをインストール
  ```cmd
  root@5b403243e9e9:/var/www/html# npm install --save-dev vue @vitejs/plugin-vue
  ```

- src/vite.config.jsを修正
  ```js
  import { defineConfig } from 'vite';
  import laravel from 'laravel-vite-plugin';
  import vue from '@vitejs/plugin-vue';
  
  export default defineConfig({
      plugins: [
          laravel({
              input: ['resources/css/app.css', 'resources/js/app.js'],
              refresh: true,
          }),
         vue(),
      ],
    server: {
      host: true,
    },
  });
  ```
