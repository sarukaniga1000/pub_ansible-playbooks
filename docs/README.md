# Kubernetes 環境構築

Ubuntu 22.04 でControl Plain/Node の準備と構築を行う

## 前提条件
- OS(Ubuntu 22.04) がインストール済みであること
- Ansible 実行ホストから各ホストへSSH できること
- 注意) Control Plain 1台, Node 2台 の構成で検証しており、LB を挟んだControl Plain 複数台の構成は検証途中
## 概要

### role

| role | 機能 |
| --- | --- |
| k8s_common | Control Plain/Node 問わず必要な設定を行う |
| k8s_master | Control Plain でのみで必要な設定を行う |
| k8s_node | Node でのみ必要な設定を行う |

OSインストール後に各ホストの役割に応じて、`k8s_master.yml`,`k8s_node.yml` を実行することで、k8s クラスタが構築可能

- 例
    ```shell
    $ ansible-playbook -i inventories/production/hosts k8s_master.yml -l ${hostname} -K
    ```

### inventory

- 接続先IP アドレスの設定必須
    - 本例では、ansible 実行ホストに`~/.ssh/config` を作成している

- 変数の設定は以下の通り
    - `inventories/production` 配下で定義

    | 変数名 | 意味 |
    | --- | --- |
    | crio_version | CRI-O のバージョン |
    | calico_version | Calico のバージョン |
    | k8s_version | Kubernetes のバージョン |
    | os_address_ipv4 | 各ホストのIPアドレス |
    | os_hostname | 各ホストのホスト名 |

# その他(参考)
環境に応じたPV,PVC の設定が追加で必要
- AWX 構築
    - playbook: [awx.yml](../awx.yml)
- Gitea 構築
    - playbook: [gitea.yml](../gitea.yml)
