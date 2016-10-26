# Install-Red5
linux下red5安装配置

###### 一、安装 JDK

    #yum install java-1.7.0-openjdk
    
###### 二、下载 解压
    //在/usr/local下安装red5
    cd /usr/local
    //下载
    wget https://github.com/Red5/red5-server/releases/download/v1.0.8-M13/red5-server-1.0.8-M13.tar.gz
    //解压到当前文件夹
    tar -zxvf red5-server-1.0.8-M13.tar.gz
    //修改文件夹名称,方便操作
    mv red5-server-1.0.8-M13 red5
    
###### 三、设置为可执行
    cd /usr/local/red5
    chmod +x *.sh

###### 四、安装
    ./red5.sh
> 如果最后一行显示：Installer service created，则说明安装成功了。此时可ctrl+c退出red5状态监测。

###### 五、编辑启动脚本
    //red5.txt在该文档所在文件夹下
    //red5启动文件，复制red5.txt到/etc/init.d/下去掉.txt
    //修改RED5_HOME=/user/local/red5
    //修改JAVA_HOME=/usr/lib/jvm-exports/java-1.7.0-openjdk-1.7.0.111.x86_64
    vi /etc/init.d/red5

    //需要对/etc/init.d/functions进行授权
    chmod +x /etc/init.d/functions
    
    //否则killproc函数无法识别，将导致无法停止red5服务
    //其它的需要授权的操作
    chmod +x /etc/rc.d/init.d/red5
    chmod +x /usr/local/red5/red5.sh
    chmod +x /usr/local/red5/red5-shutdown.sh
    
###### 六、将启动脚本添加到服务
    //添加服务
    /sbin/chkconfig --add red5
    //on表示开机自动启动 off表示开机不自动，需要手动启动
    /sbin/chkconfig red5 on
    //手动启动服务
    /sbin/service red5 start
    
###### 七、设置CentOS防火墙
    //在/etc/sysconfig/iptables文件中
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

    //下增加以下内容：
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 5080 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 1935 -j ACCEPT
    
    //重启防火墙
    /sbin/service iptables restart
    //查看端口是否开放
    /sbin/iptables -L -n
    
###### 八、测试
    在浏览器中访问 http://ip:5080
    点击 红色的Install 安装OFLA Demo
    * 注意Red5中所有Demo需要先安装后运行，在前述页面点击Install即可安装
