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


