## 基于docker实现开发环境自动搭建流程

> ### 索引
> 1. 目录结构介绍
> 2. 安装 docker
> 3. 项目配置
> 4. 安装、运行
> 5. 关于docker的一些命令
>

### 1. 目录结构介绍

```markdown
docker-compose-php/
├── configs    # 配置信息
│   ├── mysql     # mysql的配置信息
│   │    ├── conf # my.cnf配置
│   │    └── data # 数据库文件信息
│   ├── nginx
│   │    ├── conf          # my.cnf配置
│   │        ├── conf.d    # nginx默认配置
│   │        └── my.conf.d # 各应用的nginx配置
│   │    └── logs          # 数据库文件信息
│   ├── php72
│   ├── php74
│   ├── php82
├── dockerfile                  # nginx的配置和日志信息
│   ├── mongo4.4.6              # mongo dockerfile文件
│   ├── mysql-5.7
│   ├── nginx-1.19.6
│   ├── node-16.20.2
│   ├── php55-fpm
│   ├── php72-fpm
│   ├── php74-fpm
│   ├── php82-fpm
│   ├── redis-3.2.4
└── .env.example               # 环境变量
└── .docker-compose.yml        # docker-compose配置文件

```

### 2. 安装docker
>
> * [win电脑安装docker](https://docs.docker.com/desktop/setup/install/windows-install/) 
> * [Mac电脑安装Docker]

安装brew
```sh
# 安装 Homebrew，官网 brew.sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 根据输出可能需要将一两句话追加到你的 ~/.zshrc 去，可能是 ~/.bash_profile
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="/opt/homebrew/sbin:$PATH"' >> ~/.zshrc

# 重新加载 profile 文件
source ~/.zshrc  # 可能是 ~/.bash_profile

# 关于 profile 文件名的选择，和修改之后需要重新加载 profile 文件，以后会用很多次，不会再专门提及了

# 查看版本
brew -v
```

安装docker

```sh
# 安装docker
brew install docker --cask
# 安装docker-compose
brew install docker-compose
# 安装 colima  Docker Desktop的免费开源的替代工具
brew install colima
# 启动Colima
colima start
# 验证 docker 启动
docker ps
```

>
### 3. 项目配置
#### 3.1 copy根目录`.env.example`文件，将文件重命名为`.env`，配置以下目录信息
变量名| 描述            |对应的容器目录
:-|:--------------|:-:
CODE_PATH| 项目代码在你电脑的存放位置 |/data/www
CONFIG_PATH| 配置文件所在的目录     |
#### 3.2 nginx配置域名，在 configs/nginx/my.conf.d/ 新建配置文件，示例如下：
```shell script
server {
        listen       80;
        server_name  wiki.my.local;
        include dir_path.conf;
        # 设置项目路径
        root   "$my_work_path/wiki";
        index index.php index.html;
        charset utf-8;     
        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }
        location ~ \.php$ {
                # 指定PHP版本
                include  php74.conf;
        }
        location ~ /\.ht {
            deny  all;
        }
}
```
#### 3.3 域名配置，hosts示例如下
```markdown
127.0.0.1 wiki.my.local
```
### 4. 安装、运行
打开命令行工具，`切换到当前文件所在的路径`，执行以下语句：
```shell script
# 第一次会较慢，需要安装很多东西
$ docker-compose up -d
```
执行完后出现
```shell script
my-mongo       ... done
my-php72-fpm   ... done
my-redis-3.2.4 ... done
my-nginx-php   ... done
my-db          ... done
```
说明成功了，也可以执行 docker ps 查看容器
### 5. 关于docker的一些命令

更新docker-compose.yml文件配置信息
```shell script
docker-compose up --force-recreate --build -d
```