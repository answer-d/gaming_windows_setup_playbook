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
pip install ansible
```

- このリポジトリクローン

```console
git clone https://github.com/answer-d/gaming_windows_setup_playbook.git
cd gaming_windows_setup_playbook
```

- Playbook実行

```console
ansible-playbook -i inventory.yml setup.yml -u <接続ユーザ名> --ask-pass
```

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

## Refs

### Ansible公式 Windowsホストセットアップ方法

- <https://docs.ansible.com/ansible/2.9_ja/user_guide/windows_setup.html>

### Ansible Error – “NSPlaceholderDate initialize〜

- <https://rafpe.ninja/2018/02/24/ansible-error-nsplaceholderdate-initialize-may-have-been-in-progress-in-another-thread-when-fork-was-called/>
