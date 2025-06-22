# MAMPを使ったWordPressローカル開発環境構築手順

## 1\. MAMPのインストールと起動

1.  **MAMPのダウンロード:**
    MAMPの公式サイトから無料版をダウンロードし、インストールします。
    [MAMP公式サイト](https://www.mamp.info/en/downloads/)

2.  **MAMPの起動:**
    「Applications」フォルダから **MAMP** アプリケーションを起動します。

3.  **MAMPの初期設定（任意）：**
    MAMPコントロールパネルの「**Preferences**」（または「設定」）をクリックします。

      * **Ports:** 通常はデフォルトのまま (`Apache Port: 8888`, `MySQL Port: 8889`) でOKです。
      * **Web Server:** デフォルトの **Apache** で問題ありません。
      * **Document Root:** デフォルトの `/Applications/MAMP/htdocs` を使用します。変更したい場合はここで設定します。
        設定後、「**OK**」をクリックします。

4.  **サーバーの起動:**
    MAMPコントロールパネルの「**Start Servers**」ボタンをクリックします。ApacheサーバーとMySQLサーバーが起動し、ボタンが緑色になります。
    ブラウザで `http://localhost:8888` にアクセスし、MAMPのスタートページが表示されることを確認します。

-----

## 2\. データベースの作成

WordPressをインストールするための空のデータベースを作成します。

1.  **phpMyAdminの起動:**
    MAMPのスタートページ（`http://localhost:8888/MAMP/`）から、上部メニューの「**Tools**」→「**phpMyAdmin**」を選択します。

2.  **新しいデータベースの作成:**
    phpMyAdminの画面左側にある「**新規**」または「**New**」をクリックします。
    データベース名を入力します。（例: `wordpress_dev`）
    「**作成**」または「**Create**」をクリックします。

-----

## 3\. WordPress本体のGitクローン

WordPressの公式GitHubミラーをMAMPのドキュメントルートにクローンします。

1.  **ターミナルの起動:**
    「ターミナル.app」を起動します。

2.  **MAMPのドキュメントルートへの移動:**

    ```bash
    cd /Applications/MAMP/htdocs
    ```

    (MAMPのDocument Rootを変更している場合は、そのディレクトリに移動してください)

3.  **WordPressのクローン:**
    以下のコマンドでWordPressをクローンします。`your-wordpress-project-name` は、プロジェクトに付けたい名前に置き換えてください。（例: `my-wp-site`）

    ```bash
    git clone https://github.com/WordPress/WordPress.git your-wordpress-project-name
    ```

-----

## 4\. `wp-config.php`の設定

WordPressがデータベースに接続できるように設定ファイルを編集します。

1.  **プロジェクトディレクトリへの移動:**

    ```bash
    cd your-wordpress-project-name
    ```

2.  **`wp-config-sample.php`のリネーム:**

    ```bash
    mv wp-config-sample.php wp-config.php
    ```

3.  **`wp-config.php`の編集:**
    お好みのテキストエディタで `wp-config.php` ファイルを開きます。

    ```bash
    open -a "Visual Studio Code" wp-config.php # VS Codeの場合
    # または
    # open -e wp-config.php # デフォルトのテキストエディタの場合
    ```

    以下の箇所を編集します。

    ```php
    // ** MySQL settings - You can get this info from your web host ** //
    /** The name of the database for WordPress */
    define( 'DB_NAME', 'wordpress_dev' ); // ← 作成したデータベース名に変更

    /** MySQL database username */
    define( 'DB_USER', 'root' ); // ← MAMPのデフォルトは 'root'

    /** MySQL database password */
    define( 'DB_PASSWORD', 'root' ); // ← MAMPのデフォルトは 'root'

    /** MySQL hostname */
    define( 'DB_HOST', 'localhost' ); // ← そのまま
    ```

    さらに、`Authentication Unique Keys and Salts` の部分を更新します。以下のURLにアクセスし、生成されたユニークキーをコピーして貼り付けます。

    [WordPress.org Secret Key Generator](https://api.wordpress.org/secret-key/1.1/salt/)

    ファイルを保存して閉じます。

-----

## 5\. WordPressのインストール

ブラウザからWordPressにアクセスし、インストールを完了させます。

1.  **WordPressインストール画面へアクセス:**
    ブラウザで以下のURLにアクセスします。`your-wordpress-project-name`は、クローンしたディレクトリ名に置き換えてください。

    ```
    http://localhost:8888/your-wordpress-project-name/
    ```

2.  **インストールプロセスの開始:**
    「ようこそ」画面が表示されます。「**さあ、始めましょう！**」をクリックします。

3.  **サイト情報の入力:**
    サイトタイトル、管理者ユーザー名、パスワード、メールアドレスを入力します。

4.  **WordPressのインストール:**
    「**WordPressをインストール**」ボタンをクリックします。

5.  **インストール完了:**
    「成功しました！」と表示されたら完了です。「**ログイン**」をクリックして管理画面にアクセスします。

-----

## 次のステップ：テーマやプラグインのGit管理

WordPress本体は`git pull`でアップデートしていく形をとり、ご自身の開発するテーマやプラグインは別途Gitリポジトリを作成して管理します。

**例：新しいテーマを作成する場合**

1.  **テーマディレクトリへの移動:**

    ```bash
    cd /Applications/MAMP/htdocs/your-wordpress-project-name/wp-content/themes/
    ```

2.  **新しいテーマディレクトリの作成:**

    ```bash
    mkdir my-custom-theme
    ```

3.  **新しいテーマディレクトリでGitを初期化:**

    ```bash
    cd my-custom-theme
    git init
    ```

4.  **テーマファイルの作成とコミット:**
    `style.css`や`index.php`などのテーマファイルを作成し、Gitに追加・コミットします。

    ```bash
    touch style.css index.php # 例としてファイルを作成
    git add .
    git commit -m "Initial commit of my custom theme"
    ```

-----
