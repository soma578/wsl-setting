# WSL (Windows Subsystem for Linux) セットアップガイド

このガイドは、お使いのWindowsマシンにWSL (Windows Subsystem for Linux) をセットアップするための手順を説明します。

## 1. システム要件

- Windows 10 (バージョン 1607以降) または Windows 11
- コンピュータのBIOS/ファームウェア設定で仮想化が有効になっていること。

## 2. インストール

WSLをインストールする最も簡単な方法は、単一のコマンドを使用することです。

1.  **PowerShellを管理者として開く**:
    - スタートメニューをクリックし、「PowerShell」と入力します。
    - それを右クリックし、「管理者として実行」を選択します。

2.  **インストールコマンドの実行**:
    - PowerShellウィンドウで、次のコマンドを実行します:
      ```bash
      wsl --install
      ```

3.  **コンピュータを再起動する**:
    - 処理が完了したら、マシンを再起動します。

このコマンドは自動的に以下を実行します:
- 必要なWindowsの機能を有効にします（仮想マシン プラットフォームなど）。
- 最新のLinuxカーネルをダウンロードします。
- デフォルトのLinuxディストリビューションとしてUbuntuをインストールします。

## 3. 初期Linuxセットアップ

コンピュータが再起動すると、インストールが続行されます。

1.  **Linuxの起動**: 新しくインストールされたLinuxディストリビューションを初めて起動すると、コンソールウィンドウが開き、ファイルが展開されるまで数分間待機します。
2.  **ユーザーアカウントの作成**: Linuxディストリビューション用のユーザー名とパスワードを作成するように求められます。これらの資格情報は、Windowsのログイン情報と一致する必要はありません。

## 4. 別のLinuxディストリビューションのインストール (任意)

別のLinuxディストリビューションをインストールしたい場合:

1.  **利用可能なディストリビューションの一覧表示**:
    ```bash
    wsl --list --online
    ```

2.  **特定のディストリビューションのインストール**:
    ```bash
    wsl --install -d <DistroName>
    ```
    ( `<DistroName>` をリストの名前で置き換えてください)。

## 5. WSLの使用

以下の方法でLinuxディストリビューションにアクセスできます:
- スタートメニューから起動する (例: "Ubuntu")。
- PowerShellまたはコマンドプロンプトで `wsl` または `ubuntu` と入力する。

Windowsのファイルシステムは、WSL内の `/mnt/` ディレクトリからアクセスできます (例: `cd /mnt/c/Users/YourUser`)。

## 6. GitHubへのプッシュ

このガイドやWSLプロジェクトをGitHubで共有するには:

1.  [GitHub](https://github.com/new)で **新しいリポジトリを作成します** 。
2.  ローカルの `wsl-setting` ディレクトリで **Gitを初期化します**:
    ```bash
    git init
    git add .
    git commit -m "Initial commit: Add WSL setup guide"
    ```
3.  **リモートリポジトリをリンクしてプッシュします**:
    ```bash
    git remote add origin https://github.com/YourUsername/YourRepoName.git
    git branch -M main
    git push -u origin main
    ```
    ( `YourUsername` と `YourRepoName` を実際のGitHubの情報に置き換えてください)。