# Build NGINX RPM for CentOS 7



## 准备环境

1. 安装CentOS 7

2. 安装相关软件包

   ```shell
      yum groupinstall -y "Development tools"
      yum install -y epel-release
      yum install -y git
      yum install -y openssl-devel zlib-devel pcre-devel
      yum install -y wget
      yum install -y redhat-lsb-core
      yum install -y GeoIP-devel
      yum install -y gd-devel
      yum install -y libedit-devel
      yum install -y perl-devel perl-ExtUtils-Embed
      yum install -y libxslt-devel
   ```

   ​

3. 下载Nginx RPM Source

   ```shell
   wget http://hg.nginx.org/pkg-oss/archive/1.12.2-1.zip
   ```

   ​

4. 解压后将里面rpm目录里面的内容拷贝至`$HOME/rpmbuild/`



## 编译

进入目录`$HOME/rpmbuild/SPECS/`，执行`make all`即可编译完成。编译完成后，相关安装包放在：`$HOME/rpmbuild/RPMS/x86_64`



## 添加 rtmp 模块
### 下载RTMP模块源码至`$HOME/rpmbuild/SOURCES`目录

### 修改`$HOME/rpmbuild/SPECS/Makefile`

1. 在`MODULES=  geoip image-filter njs perl xslt`后面添加`rtmp`,即修改成`MODULES=   geoip image-filter njs perl xslt rtmp`

2. 在随后的行里面添加如下定义：

```makefile
MODULE_SUMMARY_rtmp= RTMP dynamic module
MODULE_VERSION_rtmp= 1.2.0
MODULE_RELEASE_rtmp=    1
MODULE_SOURCES_rtmp= rtmp-v$(MODULE_VERSION_rtmp).tar.gz
MODULE_CONFARGS_rtmp=      --add-dynamic-module=nginx-rtmp-module-$(MODULE_VERSION_rtmp)
```

3. 在后面再添加一段：

```makefile
# rtmp

define MODULE_DEFINITIONS_rtmp

endef
export MODULE_DEFINITIONS_rtmp

define MODULE_POST_rtmp
cat <<BANNER
----------------------------------------------------------------------

The $(MODULE_SUMMARY_rtmp) for nginx has been installed.
To enable this module, add the following to /etc/nginx/nginx.conf
and reload nginx:

    load_module modules/ngx_rtmp_module.so;

Please refer to the module documentation for further details:
http://nginx.org/en/docs/http/ngx_rtmp_module.html

----------------------------------------------------------------------
BANNER
endef
export MODULE_POST_rtmp
```

### 编译

在修改完Makefile之后，就按前面编译的方法编译即可。

## 安装RPM包

当将编译出来的RPM安装在一台新的机器时候，需要保证相关依赖包已经正确安装：

```shell
   yum install gd openssl openssl-libs perl libxslt
```

然后安装RPM包：

```shell
   rpm -Uvh nginx-*
```

