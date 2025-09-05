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

## 8. Ubuntuユーザー情報の変更

WSL上のUbuntuで利用するユーザー名とパスワードは、後から変更することができます。

### パスワードの変更

パスワードの変更は `passwd` コマンドで行います。

1.  Ubuntuのターミナルを開きます。
2.  以下のコマンドを実行します。
    ```bash
    passwd
    ```
3.  現在のパスワード、新しいパスワード、新しいパスワード（確認用）を順番に入力します。

#### パスワードを忘れた場合

万が一パスワードを忘れてしまった場合は、WindowsのPowerShellから以下の手順でリセットできます。

1.  PowerShellで次のコマンドを実行し、`root`ユーザーでWSLにログインします。
    ```powershell
    wsl -u root
    ```
2.  `root`としてログインしたら、次のコマンドであなたのユーザーのパスワードをリセットします。`<username>`は実際のユーザー名に置き換えてください。
    ```bash
    passwd <username>
    ```
3.  新しいパスワードを2回入力します。
4.  `exit`で`root`セッションを抜け、新しいパスワードでログインできることを確認してください。

### ユーザー名の変更

ユーザー名の変更は、現在ログインしているユーザー自身では行えないため、一時的な管理者ユーザーを作成して作業します。`<old_name>`、`<new_name>`、`<temp_user>`はご自身の環境に合わせて適宜読み替えてください。

1.  **一時ユーザーの作成と権限付与**
    ```bash
    # 一時ユーザーを作成 (パスワード設定などが求められます)
    sudo adduser <temp_user>

    # 作成した一時ユーザーに管理者権限(sudo)を付与
    sudo usermod -aG sudo <temp_user>
    ```

2.  **一時ユーザーで再ログイン**
    ```bash
    # 現在のセッションを終了
    exit
    ```
    PowerShellに戻るので、そこで以下のコマンドを実行し、一時ユーザーでWSLにログインします。
    ```powershell
    wsl -u <temp_user>
    ```

3.  **ユーザー名とホームディレクトリの変更**
    一時ユーザーでログインしている状態で、元のユーザーのユーザー名とホームディレクトリを変更します。
    ```bash
    sudo usermod -l <new_name> -d /home/<new_name> -m <old_name>
    ```

4.  **グループ名の変更**
    ユーザー名に合わせてグループ名も変更しておくと整合性が取れます。
    ```bash
    sudo groupmod -n <new_name> <old_name>
    ```

5.  **デフォルトユーザーの変更と確認**
    ```bash
    # 一時ユーザーのセッションを終了
    exit
    ```
    PowerShellで、WSL起動時のデフォルトユーザーを新しいユーザー名に設定します。
    ```powershell
    ubuntu config --default-user <new_name>
    ```
    この後、`wsl`コマンドでUbuntuを起動し、新しいユーザー名でログインできることを確認してください。

6.  **一時ユーザーの削除 (任意)**
    すべての作業が完了し、新しいユーザー名で問題なく動作することを確認したら、一時ユーザーは削除して構いません。（新しいユーザーでログインした状態で）以下のコマンドを実行します。
    ```bash
    sudo userdel -r <temp_user>
    ```