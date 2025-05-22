---
time: 2025-03-18
tags:
  - git
---
# 1 创建目录

```bash
sudo mkdir -p /home/gitlab-fox
export GITLAB_FOX_HOME=/home/gitlab-fox
```

# 2 配置新的环境变量

`$GITLAB_FOX_HOME`，设置为新目录的路径：
```bash
sudo chmod -R 777 $GITLAB_FOX_HOME
sudo mkdir -p $GITLAB_FOX_HOME/{config,logs,data}
```

# 3 Docker 部署 GitLab 的命令

```bash
sudo docker run --detach \
   --privileged \
   --hostname 192.168.30.23 \
   --env GITLAB_OMNIBUS_CONFIG="external_url 'http://192.168.30.23'" \
   --publish 9443:443 --publish 9080:80 --publish 9022:22 \
   --name gitlab \
   --restart always \
   --volume $GITLAB_FOX_HOME/config:/etc/gitlab \
   --volume $GITLAB_FOX_HOME/logs:/var/log/gitlab \
   --volume $GITLAB_FOX_HOME/data:/var/opt/gitlab \
   --shm-size 256m \
   registry.gitlab.cn/omnibus/gitlab-jh:latest
```


# 4 结果

![[Pasted image 20250320115451.png]]



