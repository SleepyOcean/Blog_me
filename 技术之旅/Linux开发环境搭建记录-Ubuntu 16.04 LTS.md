# Linux开发环境搭建记录（Ubuntu 16.04 LTS）

## 安装基本软件

1. 安装git `$ sudo apt-get install git`
2. 安装maven`$ sudo apt-get install maven``
3. 注：
   * `apt-cache search <package name>` 可查找相关包
   * 修改源的文件路径：`/etc/apt/source.list`

## 安装oracle-Java8

## 设置git

1. 修改/etc/hosts，将公司gitlab地址添加到文件中

2. 命令如下：

   ``` bash
   $ git init
   $ mkdir haiyan-services   //与远程仓库名相同
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   
   $ ssh-keygen -t rsa -C "youremail@example.com"
   
   $ git clone git@gitlab.ctsp.kedacom.com:haiyan/haiyan-services.git
   $ cd haiyan-services
   $ git checkout develop
   
   ```

