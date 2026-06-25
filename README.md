# rsync-pull-from-remote

从远端服务器通过 SSH + rsync 拉取目录到本机的 Bash 脚本。

## 依赖

```bash
sudo apt install rsync openssh-client
# 密码非交互同步还需要：
sudo apt install sshpass
```

## 快速开始

```bash
chmod +x rsync_pull_from_remote.sh

# 密钥登录（默认）
./rsync_pull_from_remote.sh -p 22 user@remote.host:/remote/path/ /local/dest/

# 自定义 SSH 端口
./rsync_pull_from_remote.sh -p 42994 root@connect.example.com:/data/project/ /home/ubuntu/project/

# 密码登录（交互式）
./rsync_pull_from_remote.sh -P -p 42994 root@connect.example.com:/remote/path/ /local/dest/

# 密码登录（非交互，通过环境变量，勿写入脚本或仓库）
SSHPASS='your_password' ./rsync_pull_from_remote.sh -P -p 42994 root@host:/remote/ /local/

# 先预览，不实际写入
./rsync_pull_from_remote.sh -n -P -p 42994 root@host:/remote/ /local/
```

## CLI 参数

| 参数 | 说明 |
|------|------|
| `<remote_spec>` | 远端路径，格式 `user@host:/path/` |
| `<local_dest>` | 本地目标目录 |
| `-p, --port` | SSH 端口，默认 22 |
| `-P, --password-auth` | 允许密码认证 |
| `-n, --dry-run` | 仅预览 |
| `-D, --delete` | 删除本地多余文件（慎用） |
| `-e, --exclude` | 排除规则，可重复 |
| `-i, --identity` | SSH 私钥路径 |

## 路径说明

- `host:/dir/` → 同步 **dir 目录内容** 到本地
- `host:/dir` → 在本地创建 **dir 子目录**

## 配置文件（可选）

```bash
cp rsync_jobs.example.conf rsync_jobs.conf
./rsync_pull_from_remote.sh --config rsync_jobs.conf --job my_job
```

## 安全建议

- **不要把密码写进脚本、配置文件或 Git 仓库**
- 优先使用 SSH 密钥：`ssh-copy-id -p PORT user@host`
- 使用 `SSHPASS` 时仅在当前 shell 临时设置，同步完成后 `unset SSHPASS`

## License

MIT
