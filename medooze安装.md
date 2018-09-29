
# mcu media server centos7 安装
https://docs.google.com/document/d/1q4aFgk2DYhZmB_rwo61fq1Nsihne7Fz1jUNb5sBZdKc/edit


1.打开两个终端，Apache Tomcat SipServlet需要花费大量时间(大约60分钟)来下载和编译。

因此，您可以花时间让一个终端执行此任务，并打开另一个终端，并跟随本教程的下一部分。

* root 权限运行以下命令
  
  ## 关闭防火墙
  
  *  systemctl disable firewalld
  *  vim  /etc/selinux/config
  
  ```
  修改
  SELINUX=enforcing 
  为
  SELINUX=disabled
  保存退出
  ```
## 安装Apache TOMCAT SIP-SERVLET

* yum install -y maven git ant subversion
* cd /usr/local/src
*  git clone https://github.com/RestComm/sip-servlets.git
*  cd sip-servlets
*  cd ./build/release
*  ant -buildfile ./build.xml
这里会安装60分钟左右所以我们在另一个终端上运行其他命令

# java 安装
* yum install java-1.8.0-openjdk-devel
* alternatives --config javac (选择你安装的版本)
# XMLRPC 安装
* cd /usr/local/src
* wget https://archive.apache.org/dist/ws/xmlrpc/binaries/apache-xmlrpc-3.1.3-bin.tar.bz2  
* wget http://dlc-cdn.sun.com/javaee5/external/shared/ssa-api/jars/ssa-api-1.1.jar
* tar xvjf apache-xmlrpc-3.1.3-bin.tar.bz2
* svn checkout https://svn.code.sf.net/p/mcumediaserver/code/trunk mcumediaserver-code
* cd mcumediaserver-code
* rm -rf mcu sdp
*  git clone https://github.com/medooze/sdp.git
*  git clone https://github.com/medooze/media-server.git
* 修改 mcumediaserver-code/XmlRpcMcuClient/nbproject/project.properties 以下部分
  ```
javac.source=1.8
javac.target=1.8
platform.active=JDK_1.8
file.reference.commons-logging-1.1.jar=/usr/local/src/apache-xmlrpc-3.1.3/lib/commons-logging-1.1.jar
file.reference.ws-commons-util-1.0.2.jar=/usr/local/src/apache-xmlrpc-3.1.3/lib/ws-commons-util-1.0.2.jar
file.reference.xmlrpc-client-3.1.3.jar=/usr/local/src/apache-xmlrpc-3.1.3/lib/xmlrpc-client-3.1.3.jar
file.reference.xmlrpc-common-3.1.3.jar=/usr/local/src/apache-xmlrpc-3.1.3/lib/xmlrpc-common-3.1.3.jar
  
  ```
  ## sdp 安装
  
* cd /usr/local/src/mcumediaserver-code/sdp/
* nbproject/project.properties 以下部分
  
  ```
  主要是java版本号
  ```
  ## mcuWeb 安装
* cd /usr/local/src/

* git clone https://github.com/primoitt83/javamail.git

* cd /usr/local/src/javamail

* unzip javamail1_4_5.zip
* cat sailfin-installer-v2-b31g-linux_part* > sailfin-installer-v2-b31g-linux.jar
*  cat netbeans8_part* > netbeans8.tar
*   tar -zxvf netbeans8.tar
* java -Xmx256m -jar sailfin-installer-v2-b31g-linux.jar
  
  ```
  你选择a就行
  ```
* mv sailfin/ /usr/local/
* mv netbeans /usr/share
* mv servlet-api-2.5.jar /usr/share/java
* mv /usr/local/src/javamail/javamail-1.4.5/mail.jar /usr/share/java
  
*  cd /usr/local/sailfin
*  chmod -R +x lib/ant/bin
*  vim setup.xml 修改Java版本
 
  ```
       	<contains string="${targeted.java.version}" substring="1.5"/>
     	<contains string="${targeted.java.version}" substring="1.6"/>
     	<contains string="${targeted.java.version}" substring="1.8"/>
  ```
* ant -f setup.xml
* cd /usr/local/src
* wget download.java.net/glassfish/4.0/release/glassfish-4.0.zip
* unzip glassfish-4.0.zip -d /opt
* cd /usr/local/src/mcumediaserver-code/mcuWeb/
  
  ```
  # perl -pi -e 's/file.reference.commons-logging-1.1.jar=.*$/file.reference.commons-logging-1.1.jar=\/usr\/local\/src\/apache-xmlrpc-3.1.3\/lib\/commons-logging-1.1.jar/' nbproject/project.properties 


# perl -pi -e 's/file.reference.ssa-api-1.1.jar=.*$/file.reference.ssa-api-1.1.jar=\/usr\/local\/src\/ssa-api-1.1.jar/' nbproject/project.properties 

# perl -pi -e 's/file.reference.ws-commons-util-1.0.2.jar=.*$/file.reference.ws-commons-util-1.0.2.jar=\/usr\/local\/src\/apache-xmlrpc-3.1.3\/lib\/ws-commons-util-1.0.2.jar/' nbproject/project.properties


# perl -pi -e 's/file.reference.xmlrpc-client-3.1.3.jar=.*$/file.reference.xmlrpc-client-3.1.3.jar=\/usr\/local\/src\/apache-xmlrpc-3.1.3\/lib\/xmlrpc-client-3.1.3.jar/' nbproject/project.properties


# perl -pi -e 's/file.reference.xmlrpc-common-3.1.3.jar=.*$/file.reference.xmlrpc-common-3.1.3.jar=\/usr\/local\/src\/apache-xmlrpc-3.1.3\/lib\/xmlrpc-common-3.1.3.jar/' nbproject/project.properties


# perl -pi -e 's/file.reference.XmlRpcMcuClient.jar=.*$/file.reference.XmlRpcMcuClient.jar=\/usr\/local\/src\/mcumediaserver-code\/XmlRpcMcuClient\/dist\/XmlRpcMcuClient.jar/' nbproject/project.properties

# perl -pi -e 's/javac.target=1.6.*$/javac.target=1.7/' nbproject/project.properties

# nano nbproject/project.properties

Look for:

javac.classpath=${file.reference.XmlRpcMcuClient.jar}\:${file.reference.commons-logging-1.1.jar}\:${file.reference.ws-commons-util-1.0.2.jar}\:${file.reference.xmlrpc-client-3.1.3.jar}\:${file.reference.xmlrpc-common-3.1.3.jar}\:${reference.sdp.jar}\:${file.reference.ssa-api-1.1.jar}

Change to:

javac.classpath=\
   /usr/share/java/mail.jar:\
   /usr/share/java/servlet-api-2.5.jar:\
   /opt/glassfish4/glassfish/modules/javax.persistence.jar:\
   ${file.reference.XmlRpcMcuClient.jar}:\
   ${file.reference.commons-logging-1.1.jar}:\
   ${file.reference.ws-commons-util-1.0.2.jar}:\
   ${file.reference.xmlrpc-client-3.1.3.jar}:\
   ${file.reference.xmlrpc-common-3.1.3.jar}:\
   ${reference.sdp.jar}:\
   ${file.reference.ssa-api-1.1.jar}


# nano /usr/local/src/mcumediaserver-code/mcuWeb/web/WEB-INF/jboss-web.xml



Look for:

<?xml version="1.0" encoding="UTF-8"?>
<jboss-web>
  <context-root/>
</jboss-web>

Change to:

<?xml version="1.0" encoding="UTF-8"?>
<jboss-web>
<context-root>/mcuWeb</context-root>
</jboss-web>

# ant -Dplatforms.JDK_1.7.home=$JAVA_HOME -Dj2ee.server.home=/usr/local/sailfin -Dlibs.CopyLibs.classpath=/usr/share/netbeans/java/ant/extra/org-netbeans-modules-java-j2seproject-copylibstask.jar

Results expected:

  ```
  *  ls /usr/local/src/mcumediaserver-code/mcuWeb/dist/mcuWeb.sar -lah
  *  yum install -y yasm git automake build-essential ant autoconf unzip nano openssl-devel nss-devel libpng-devel libgcrypt-devel libsrtp-devel libjpeg-turbo-devel gsm-devel libcurl-devel subversion cmake freetype-devel gcc gcc-c++ libtool make mercurial nasm pkgconfig zlib-devel

* cd /usr/local/src


* wget http://downloads.xiph.org/releases/speex/speex-1.2.0.tar.gz

* wget http://downloads.xiph.org/releases/opus/opus-1.1.4.tar.gz

* wget http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz

* git clone https://github.com/TechSmith/mp4v2.git

* wget http://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2

* wget http://download.videolan.org/pub/x264/snapshots/x264-snapshot-20170505-2245-stable.tar.bz2

* wget http://storage.googleapis.com/downloads.webmproject.org/releases/webm/libvpx-1.6.1.tar.bz2

* git clone https://github.com/cisco/libsrtp

* git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git 
编译安装

* cd /usr/local/src

## YASM
 ```
 tar -zxvf yasm-1.3.0.tar.gz
 cd yasm-1.3.0
 ./configure
 make
 make install
 cd ..
 ```
## X264

```
    sudo git clone git://git.videolan.org/x264.git
cd /usr/local/src/x264
    ../configure --enable-static --enable-shared --enable-pic
    make
    make install
    cd ..
```

## FFMPEG 安装

```
    sudo git clone git://git.videolan.org/ffmpeg.git
cd /usr/local/src/ffmpeg
sudo git remote set-url origin git://source.ffmpeg.org/ffmpeg
sudo ./configure --prefix=/usr --enable-zlib --disable-stripping --enable-nonfree
--enable-gpl --enable-shared --enable-pic --enable-avresample --enable-decoder=png
sudo make
sudo make install
    cd ..
```

## XMLRPC-C

```
    cd /usr/local/src/javamail
    tar -zxvf xmlrpc-c-1.39.12.tgz
    cd xmlrpc-c-1.39.12
    ./configure
    make
    make install
    cd /usr/local/src
```

## MP4V2

```
 cd mp4v2
 autoreconf -fiv
 ./configure
 make
 make install 
 cd ..

```


## VP8/VP9 安装

```
 tar -jxvf libvpx-1.6.1.tar.bz2
 cd libvpx-1.6.1
 ./configure --enable-shared
 make
 make install
 cd ..

```

## SPEEX

```
tar -zxvf speex-1.2.0.tar.gz
cd speex-1.2.0
./configure
make
make install
cd ..
```


## OPUS
```
    tar -zxvf opus-1.1.4.tar.gz
    cd opus-1.1.4
    ./configure
    make
    make install
    cd ..
```


## SRTP

```
    cd libsrtp
    ./configure
    make
    make install
    cd ..
```

## VAD 安装

```
    export PATH="$PATH":/usr/local/src/depot_tools
    cd /usr/local/src/mcumediaserver-code/media-server/ext
    ninja -C out/Release/ common_audio

````

## tomcat 安装

* mv /usr/local/src/sip-servlets/build/release/target/apache-tomcat-8.0.26 /usr/local
* cd /usr/local

* mv apache-tomcat-8.0.26/ tomcat/
* cp /usr/local/src/mcumediaserver-code/mcuWeb/dist/mcuWeb.sar /usr/local/tomcat/webapps/mcuWeb.war
