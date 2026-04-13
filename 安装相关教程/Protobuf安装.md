[https://cloud.tencent.com/developer/article/2523477](https://cloud.tencent.com/developer/article/2523477)

```shell
1. sudo apt-get install autoconf automake libtool curl make g++ unzip -y    # ubuntu命令
2. wget https://github.com/protocolbuffers/protobuf/releases/download/v21.11/protobuf-all-21.11.zip
3. unzip protobuf-all-21.11.zip
4. cd protobuf-21.11
```

```shell
# 第一步：执行./autogen.sh	（如果下载的是某一具体语言的版本，则不需要这一步）
./autogen.sh		

# 第二步：执行configure，有两种执行⽅式，任选其⼀即可，如下：
# 	1、protobuf默认安装在 /usr/local ⽬录，lib、bin都是分散的
./configure
#	2、修改安装目录，统一安装在/usr/local/protobuf下
./configure --prefix=/usr/local/protobuf
```

```shell
make 		# 执⾏15分钟左右
make check 	 # 执⾏15分钟左右
sudo make install
```



```shell
	sudo vim /etc/profile

# 添加内容如下：
#	(动态库搜索路径) 程序加载运行期间查找动态链接库时指定除了系统默认路径之外的其他路径
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/protobuf/lib/
#	(静态库搜索路径) 程序编译期间查找动态链接库时指定查找共享库的路径
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/protobuf/lib/
#	执⾏程序搜索路径
export PATH=$PATH:/usr/local/protobuf/bin/
#	c程序头文件搜索路径
export C_INCLUDE_PATH=$C_INCLUDE_PATH:/usr/local/protobuf/include/
#	c++程序头文件搜索路径
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/local/protobuf/include/
#	pkg-config路径
export PKG_CONFIG_PATH=/usr/local/protobuf/lib/pkgconfig/
```

```shell
source  /etc/profile 
```

