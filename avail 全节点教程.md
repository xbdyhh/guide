# AVAIL 全节点教程

官方文档地址：

https://docs.availproject.org/

### 硬件要求

|                      | 最小配置 | 推荐配置   |
| -------------------- | -------- | ---------- |
| 内存                 | 4GB      | 8GB        |
| CPU (amd64/x86 架构) | 2 core   | 4 core     |
| 硬盘 (SSD)           | 20-40 GB | 200-300 GB |

本教程系统： unbuntu22.04

#### 下载相关插件

```
sudo apt-get update
sudo apt install build-essential
cd ~
wget https://github.com/availproject/avail/releases/download/v1.8.0.3/x86_64-ubuntu-2204-data-avail.tar.gz
tar -zxvf x86_64-ubuntu-2204-data-avail.tar.gz
sudo mv data-avail /usr/local/bin/data-avail
sudo chmod +x /usr/local/bin/data-avail
```

运行

```
data-avail -V
```

你应该看到

```
data-avail 1.8.3-a0820931850
```

### 通过预编译的程序运行节点

```
sudo tee /etc/systemd/system/availd.service > /dev/null <<EOF
[Unit]
Description=availd
After=network-online.target
[Service]
User=$USER
ExecStart=/usr/local/bin/data-avail --base-path `pwd`/data --chain kate --name "<你的节点名称>"
Restart=always
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
#启动服务
sudo systemctl daemon-reload && \
sudo systemctl enable availd && \
sudo systemctl restart availd
sudo journalctl -u availd -f
```

你应该看到

```
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [10127] 💸 generated 13 npos targets
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [10127] 💸 generated 14 npos voters, 13 from validators and 1 nominators
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [#10127] 🗳  creating a snapshot with metadata SolutionOrSnapshotSize { voters: 14, targets: 13 }
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [#10127] 🗳  Starting phase Signed, round 10.
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [#10172] 🗳  Starting phase Unsigned((true, 10172)), round 10.
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [#10173] 🗳  queued unsigned solution with score ElectionScore { minimal_stake: 2290845780262072, sum_stake: 16299332905707287, sum_stake_squared: 37964860009725128956655154626195 }
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [#10217] 🗳  Finalized election round with compute Unsigned.
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [#10217] 🗳  Starting phase Off, round 11.
Nov 02 08:47:21 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:21 [10217] 💸 new validator set of size 7 has been processed for era 10
Nov 02 08:47:24 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:24 [11207] 💸 generated 14 npos targets
Nov 02 08:47:24 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:24 [11207] 💸 generated 16 npos voters, 14 from validators and 2 nominators
Nov 02 08:47:24 Ubuntu-2204-jammy-amd64-base data-avail[4135660]: 2023-11-02 08:47:24 [#11207] 🗳  creating a snapshot with metadata SolutionOrSnapshotSize { voters: 16, targets: 14 }

```

等待节点同步，大约耗时1个小时左右。

之后你可以在：节点面板看到你的节点：https://telemetry.avail.tools/#/0xd12003ac837853b062aaccca5ce87ac4838c48447e41db4a3dcfb5bf312350c6
