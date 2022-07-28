# 2022-03-10

## conda 无法在命令行识别
通过添加环境变量到 Windows 系统

1. 在搜索中输入 `env` 点击编辑**系统**环境变量
2. 在 `PATH` 中新增一条 `C:\Users\你的用户名\anaconda3\Scripts`

至此，`conda` 命令就可在 Windows 的命令行，如 PowerShell 中被识别

## conda create 无法从源获取软件包
首先，以下设置都是在 `.condarc` 文件中修改，该文件在 `C:\Users\你的用户名` 中

### 第一次尝试：改变网络环境
设法令系统连接到国际互联网

格式如下

```yaml
proxy_servers:
    http: http://username:password@corp.com:8080
    https: https://username:password@corp.com:8080
```

参考
[Anaconda文档](https://docs.anaconda.com/anaconda/user-guide/tasks/proxy/)

失败

#### 关于如何找到部分 VPN 的代理地址
去浏览器的设置中找到代理地址

### 第二次尝试：修改软件包的托管源
尝试改成阿里源

参考
[CSDN](https://blog.csdn.net/weixin_43667077/article/details/106521015)

成功


## 遗留问题
似乎，仅仅添加 `C:\Users\你的用户名\anaconda3\Scripts` 到 `PATH` 仍然无法识别命令 `conda activate`

[x] 未解决



## 如何进入cmd
1. 新建终端
2. 更换为`command prompt`



## cmd(而非powershell)
1. 以管理员身份运行
2. 在终端输入`conda activate data`(data也仅仅是一个环境名)
3. 接着`conda install numpy`(numpy仅仅是一个软件包名)

至此，软件包安装完成


## 在环境中编译运行

### 如果在配好的环境中
1. 输入`python 文件名`
2. 回车

### 如果没有在配好的环境中
1. 在终端中输入`conda activate data`
2. 接下来操作如上

#### tip:可用tab键补全


### 无法识别conda activate
1. 在开始里打开anaconda prompt
2. 在那个里面创建环境
3. 敲入`code .`
4. 打开VScode以后，不出意外，右下角的环境为新建立的，如果不是，那就是出意外了


### 关于conda activate 
- 把`C:\Anaconda\envs\py3`加入到`path`

