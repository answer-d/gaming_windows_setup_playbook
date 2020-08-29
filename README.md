# 俺のゲーミングWindowsを構築するAnsible Playbook

## Installation

### Windowsでの事前作業

- IPアドレスを設定して控えておく

- WinRM有効化  

    ```powershell
    Invoke-WebRequest -Uri https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1 -OutFile ConfigureRemotingForAnsible.ps1
    powershell -ExecutionPolicy RemoteSigned .\ConfigureRemotingForAnsible.ps1
    ```

### 何かLinux環境での作業

Windowsしかない場合は…VMとかDockerで頑張って(投げやり)  

- Ansible実行環境を用意する  
    例えばpython3入ってる環境ならこんな感じ  

    ```console
    python -m venv .venv
    source .venv/bin/activate
    pip install -r python_requirements.txt
    ```

- このリポジトリクローン  

    ```console
    git clone https://github.com/answer-d/gaming_windows_setup_playbook.git
    cd gaming_windows_setup_playbook
    ```

- collectionインストール  

    ```console
    ansible-galaxy collection install -r ansible_requirements.yml -p .ansible/collections
    ```

- Playbook実行  

    ```console
    ansible-playbook setup.yml -u <接続ユーザ名>
    ```

    接続ユーザは管理者権限を持つこと

- こんな感じのエラーが出たら  

    ```plain
    objc[6760]: +[__NSPlaceholderDate initialize] may have been in progress in another thread when fork() was called.
    objc[6760]: +[__NSPlaceholderDate initialize] may have been in progress in another thread when fork() was called. We cannot safely call it or ignore it in the fork() child process. Crashing instead. Set a breakpoint on objc_initializeAfterForkError to debug.
    ```

    - これで回避できるはず  

        ```console
        export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
        ```

## Memo

- Playbook実行時に毎回パスワード打つの面倒だし証明書で認証する方式やってみたい
    - <https://docs.ansible.com/ansible/2.9_ja/user_guide/windows_winrm.html#id4>
    - めんどいので保留なう
- Chocolateyでインストールできないものたち
    - Blitz.gg
        chocoパッケージ無し
    - Razerのやつ(Razer Central？)
        - chocoパッケージ無し
        - Synapseはあったけど多分古い
    - google ime
        - 多分インストール後にウインドウ出てくるから？失敗したっぽい
        - Windowsログイン後に `choco install googlejapaneseinput` でインストールしよう
- [gitconfigモジュール](https://docs.ansible.com/ansible/latest/modules/git_config_module.html)はLinuxにしか使えないのであきらめた
    - まぁ別に開発に使わんからええやろ！(適当)

## Refs

### Ansible公式 Windowsホストセットアップ方法

- <https://docs.ansible.com/ansible/2.9_ja/user_guide/windows_setup.html>

### Ansible Error – “NSPlaceholderDate initialize〜

- <https://rafpe.ninja/2018/02/24/ansible-error-nsplaceholderdate-initialize-may-have-been-in-progress-in-another-thread-when-fork-was-called/>

### Windows 10 Proの初期設定をAnsibleなどでやってみる

- <https://qiita.com/yunano/items/8f1db93dc34f7aeeb469>
