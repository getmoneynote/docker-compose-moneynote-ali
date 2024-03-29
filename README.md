# MoneyNote 个人部署方案

本项目提供docker compose一键运行 MoneyNote，搭建自己的记账环境。

适合所有能安装docker的机器运行，支持amd和arm架构。

如遇到任何问题欢迎加入 QQ群: 639653091 讨论。

#### 部署外网环境请注意：
1. 镜像的mysql服务root用户的默认密码为78p7gkc1，请修改root默认密码。
2. 为防止恶意注册，请修改注册邀请码，invite_code参数。

### 快速启动

```sh
docker run --name moneynote -e DB_PASSWORD=78p7gkc1 -e invite_code=111111 -v moneynote_mysql_data:/var/lib/mysql -p 43740:3306 -p 43741:80 -p 43742:9092 -p 43743:81 -p 43744:82 registry.cn-hangzhou.aliyuncs.com/moneynote/moneynote-all:latest
```

如果已有mysql服务，可使用不带mysql的镜像启动。

```sh
docker run --name moneynote -d \
	-e DB_HOST=your_ip \
	-e DB_PORT=3306 \
	-e DB_NAME=moneynote \
	-e DB_USER=root \
    -e DB_PASSWORD=your_password \
	-e invite_code=111111 \
	-p 43742:9092 \
	-p 43743:81 \
	-p 43744:82 \
	registry.cn-hangzhou.aliyuncs.com/moneynote/moneynote-all-no-mysql:latest
```

### docker compose 启动（推荐）

1. 请下载本项目源代码，使用git命令或直接下载源代码。

```sh
git clone https://github.com/getmoneynote/docker-compose-moneynote-ali.git && cd docker-compose-moneynote-ali
```

2. docker compose 启动

```sh
docker compose up -d
```

3. 升级

```sh
docker compose pull && docker compose up -d
```

成功运行后，访问 [http://127.0.0.1:43743](http://127.0.0.1:43743) 可以打开网页版记账程序，使用前请注册一个账户，默认的邀请码是111111（6个1）, 为防止被恶意注册，请修改默认邀请码。

使用手机浏览器访问，[http://127.0.0.1:43744](http://127.0.0.1:43744) （127.0.0.1替换成你的地址）。

如需备份数据，请访问 [http://127.0.0.1:43741](http://127.0.0.1:43741) 打开phpMyAdmin操作，直接导出SQL数据。

phpMyAdmin登录的信息请对照api.env配置文件填写。请定期使用phpMyAdmin导出sql文件，备份你的记账数据！！！！！！！！


#### docker命令说明

with mysql 启动  （支持arm）
```sh
docker compose --env-file api.env -f docker-compose-ali.yml up -d
```

with mysql 升级
```sh
docker compose --env-file api.env -f docker-compose-ali.yml pull && docker compose --env-file api.env -f docker-compose-ali.yml up -d
```

no mysql 启动
```sh
docker-compose --env-file api-no-mysql.env -f docker-compose-ali-no-mysql.yml up -d
```

no mysql 升级
```sh
docker compose --env-file api-no-mysql.env -f docker-compose-ali-no-mysql.yml pull && docker-compose --env-file api-no-mysql.env -f docker-compose-ali-no-mysql.yml up -d
```

docker 5 in 1 启动
```sh
docker compose --env-file api.env -f docker-compose-all-ali.yml up -d
```

docker 5 in 1 升级
```sh
docker compose -f docker-compose-all-ali.yml pull && docker compose --env-file api.env -f docker-compose-all-ali.yml up -d
```

docker 3 in 1 启动
```sh
docker compose --env-file api-no-mysql.env -f docker-compose-all-no-mysql-ali.yml up -d
```

docker 3 in 1 升级
```sh
docker compose -f docker-compose-all-no-mysql-ali.yml pull && docker compose --env-file api-no-mysql.env -f docker-compose-all-no-mysql-ali.yml up -d
```


## QA
1. 很多人安装遇到数据库的问题，有可能是之前安装过，有数据文件，且自己修改过root密码。 使用 docker volume ls 命令查看有没有moneynote_mysql_data文件，如果有，可以自己修改为另外的数据文件，或者删除moneynote_mysql_data

## 参考资料
[安装docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-centos-7)