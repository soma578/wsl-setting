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

### インストール時のトラブルシューティング

`wsl --install` の実行中にエラーが発生した場合は、以下の点を確認してください。

#### 1. 仮想化が有効になっていない

**エラー例:** `WSL 2 requires an update to its kernel component.` や `Please enable the Virtual Machine Platform Windows feature and ensure virtualization is enabled in the BIOS.` (エラーコード: `0x80370102` など)

**解決策:**

1.  **BIOS/UEFIで仮想化を有効にする:**
    *   PCを再起動し、起動中に `F2`、`Del` などのキーを押してBIOS/UEFI設定画面を開きます。
    *   `Advanced`, `CPU Configuration` といった項目の中に `Intel (R) Virtualization Technology (VT-x)` や `AMD-V (SVM Mode)` のような設定項目があります。これを `Enabled` (有効) に変更して保存し、再起動します。

2.  **Windowsの機能を有効にする:**
    *   PowerShellを **管理者として** 実行し、以下のコマンドを実行します。
      ```powershell
      dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
      dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
      ```
    *   完了後、PCを再起動します。

#### 2. WSLカーネルが古い

**エラー例:** `WSL 2 requires an update to its kernel component.`

**解決策:**

1.  PowerShellを **管理者として** 実行し、以下のコマンドでWSLを更新します。
    ```powershell
    wsl --update
    ```
2.  PCを再起動し、再度インストールを試みます。

#### 3. その他のエラー (エラーコード: `0x80070003` など)

**解決策:**

*   **Windowsのバージョンを確認する:** `winver` コマンドで、Windows 10 バージョン 2004 以降、またはWindows 11であることを確認してください。
*   **Windows Updateを実行する:** システムが最新の状態でない場合にエラーが起きることがあります。
*   **ストアアプリの保存先を確認する:** **設定 > システム > 記憶域 > 詳細ストレージ設定 > 新しいコンテンツの保存先** を開き、「新しいアプリの保存先」がシステムドライブ（通常は `C:`）になっていることを確認します。

## 3. 初期Linuxセットアップ

コンピュータが再起動すると、インストールが続行されます。

1.  **Linuxの起動**: 新しくインストールされたLinuxディストリビューションを初めて起動すると、コンソールウィンドウが開き、ファイルが展開されるまで数分間待機します。
2.  **ユーザーアカウントの作成**: Linuxディストリビューション用のユーザー名とパスワードを作成するように求められます。これらの資格情報は、Windowsのログイン情報と一致する必要はありません。

## 4. WSLの再インストール

問題が解決しない場合や、クリーンな状態からやり直したい場合は、WSLを再インストールすることができます。

### 手順1: Linuxディストリビューションの登録を解除する

まず、インストールされているすべてのLinuxディストリビューションの登録を解除（アンインストール）します。

1.  PowerShellを開き、インストール済みのディストリビューション一覧を確認します。
    ```powershell
    wsl --list --all
    ```

2.  一覧に表示された各ディストリビューションを、以下のコマンドで登録解除します。
    ```powershell
    wsl --unregister <DistroName>
    ```
    (例: `wsl --unregister Ubuntu`)

### 手順2: WSL関連機能の無効化と有効化

次に、WSLに関連するWindowsの機能を一度無効化し、再度有効化します。

1.  PowerShellを **管理者として** 実行し、以下のコマンドで機能を無効化します。
    ```powershell
    dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux /norestart
    dism.exe /online /disable-feature /featurename:VirtualMachinePlatform /norestart
    ```

2.  PCを再起動します。

3.  再度PowerShellを **管理者として** 実行し、機能を有効化します。
    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```

4.  もう一度PCを再起動します。

### 手順3: WSLの再インストール

最後に、`wsl --install` コマンドで再度インストールを実行します。

```powershell
wsl --install
```

これで、WSLがクリーンな状態で再インストールされます。

## 5. 別のLinuxディストリビューションのインストール (任意)

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

## 6. WSLの使用

以下の方法でLinuxディストリビューションにアクセスできます:
- スタートメニューから起動する (例: "Ubuntu")。
- PowerShellまたはコマンドプロンプトで `wsl` または `ubuntu` と入力する。

Windowsのファイルシステムは、WSL内の `/mnt/` ディレクトリからアクセスできます (例: `cd /mnt/c/Users/YourUser`)。

## 7. GitHubへのプッシュ

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