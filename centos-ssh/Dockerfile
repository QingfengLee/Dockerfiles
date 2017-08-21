FROM hub.c.163.com/library/centos:7.2.1511

MAINTAINER liqingfeng liqingfengzzu@163.com

#替换为网易源
ADD CentOS7-Base-163.repo /etc/yum.repos.d/CentOS-Base.repo

#安装常用软件
RUN yum install -y sudo vim tar wget curl rsync bzip2 iptables tcpdump less net-tools lsof sysstat cronie passwd openssl openssh-server openssh-clients screen strace telnet unzip python-setuptools\
&& yum clean all

#生成ssh所需要的部分内容
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key -N ''
RUN ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
RUN ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ''
#允许RSA认证
RUN echo "RSAAuthentication yes" >> /etc/ssh/sshd_config
# 允许公钥认证
RUN echo "PubkeyAuthentication yes" >> /etc/ssh/sshd_config 

# 安装supervisor
RUN easy_install supervisor\
 && cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime\
 && mkdir -p /etc/supervisor/conf.d/\
 && /usr/bin/echo_supervisord_conf > /etc/supervisord.conf\
 && echo [include] >> /etc/supervisord.conf\
 && echo 'files = /etc/supervisor/conf.d/*.conf' >> /etc/supervisord.conf\
 && yum clean all

#ssh的supervisor配置
COPY sshd.conf /etc/supervisor/conf.d/sshd.conf

#设置密码
RUN echo 'root:123@qwe' | chpasswd 
#暴露22端口
EXPOSE 22
#启动
CMD ["/usr/bin/supervisord"]

