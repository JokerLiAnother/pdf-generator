# Getting Started

当前项目通过启动`web`服务，来提供动态生成PDF文件的接口服务。使用
[go-wkhtmltopdf](github.com/SebastiaanKlippert/go-wkhtmltopdf)来制作`PDF`文件,
需要在目标机器上安装[wkhtmltopdf]( http://wkhtmltopdf.org/index.html )命令。

## 安装`wkhtmltopdf` **CentOS** 为例

首先安装依赖库
```shell
yum install fontconfig libX11 libXext libXrender libjpeg libpng xorg-x11-fonts-75dpi xorg-x11-fonts-Type1
yum install openssl
```


下载最新版本`wkhtmltopdf`的二进制文件并安装

```shell
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox-0.12.6-1.centos7.x86_64.rpm

rpm -ivh wkhtmltox-0.12.6-1.centos7.x86_64.rpm 
```
测试命令
```shell
whereis wkhtmltopdf

# wkhtmltopdf: /usr/local/bin/wkhtmltopdf

/usr/local/bin/wkhtmltopdf www.google.com a.pdf
```



## 配置文件`.pdf-generator.yaml`

在启动项目前需要在配置文件中配置必要参数，服务默认端口为`7006`, 文件缓存路径默认为`/tmp/pdf-generator/`
，如果没有自定义文件路径，启动前请保证该路径已存在。
数据库连接为必要参数，需要补充数据库访问连接`USERNAME:PASSWORD@tcp(HOST:PORT)/DATABASE`

```yaml
port: 7006

# tmp dir
tmp-dir: /tmp
mysql:
  driver: mysql
  url: 'USERNAME:PASSWORD@tcp(HOST:PORT)/DATABASE'
  # connection max lifetime: default 3 minutes
  max-life-time: 3
  # max open connections: default 10
  max-open-connections: 10
  # max idle connections: default 10
  max-idle-connections: 10
```

## 启动

应用为二进制文件，通过命令行启动

```shell
pdf-generator --config .pdf-generator.yaml
```

## 接口

### 制作指定类型的`pdf`文件

```http request
## 下载 vat-note pdf 文件
GET https://localhost:7006/generatePdf/vat-note/CUSTOMSID

```
