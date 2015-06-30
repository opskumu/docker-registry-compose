# Docker Compose V1 + V2 registry

## 一、Install Docker Compose

```
wget https://github.com/docker/compose/releases/download/1.3.1/docker-compose-Linux-x86_64
mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
```

## 二、SSL 证书

相关证书需要你自己重新签名[替换自带的 test.key，根据实际情况填写]，SSL 证书生成方式如下：

```
# openssl req -newkey rsa:4096 -nodes -sha256 -keyout test.key -x509 -days 10000 -out test.crt
Generating a 4096 bit RSA private key
..................................................................................................................................................................................++
...++
writing new private key to 'test.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:Shanghai
Locality Name (eg, city) [Default City]:Shanghai
Organization Name (eg, company) [Default Company Ltd]:TEST
Organizational Unit Name (eg, section) []:TEST
Common Name (eg, your name or your server's hostname) []:www.test.com    # 此处填写主机名
Email Address []:admin@test.com
```

__注：__ CentOS6.6 上 Docker1.5 版本在登录时出现证书认证错误，这时需要把相关证书加入到系统认证即可，CentOS7 没有类似问题。

```
# docker login -u test -p 123321 -e admin@test.com www.test.com
FATA[0000] Error response from daemon: Server Error: Post https://www.test.com/v1/users/: x509: certificate signed by unknown authority
```

把生成的 `test.crt` 文件内容追加到 `/etc/pki/tls/certs/ca-bundle.crt` 即可。[修改完成之后需要重启 docker]

* `.htpasswd` 文件也需要你使用 `htpasswd` 命令重新修改添加对应的用户和密码。[本例为 test/123321]
* `registry.conf` 配置文件中 nginx 域名修改你实际情况下的访问域名，和 CA 证书 hostname 一致

## 三、Build and run with Compose

clone 之后在 `docker-compose.yml` 所在目录执行 `docker-compose build` 构建 Nginx 镜像

```
docker-compose build    # 构建 Nginx 镜像
docker-compose run -d   # 运行 Registry
```

## 四、docker-compose 用法  

```
# docker-compose -h
Define and run multi-container applications with Docker.

Usage:
  docker-compose [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE           Specify an alternate compose file (default: docker-compose.yml)
  -p, --project-name NAME   Specify an alternate project name (default: directory name)
  --verbose                 Show more output
  -v, --version             Print version and exit

Commands:
  build              Build or rebuild services
  help               Get help on a command
  kill               Kill containers
  logs               View output from containers
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pulls service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  up                 Create and start containers
  migrate-to-labels  Recreate containers to add labels
```
