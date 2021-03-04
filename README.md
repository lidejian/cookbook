# 目录
[toc]

# python

## 学习资源

* 实践指南   https://pythonguidecn.readthedocs.io/zh/latest/
* cookbook  https://python3-cookbook.readthedocs.io/zh_CN/latest/

## anaconda环境

* 创建环境

  ```python
  # 代表创建一个python3.6的环境，我们把它命名为python36
  conda create --name python36 python=3.6
  
  # 查看环境
  conda info --env                               
  
   # 删除环境
  conda remove -n python36 --all       		 
  ```

* 将conda环境写入notebook

  ```python
  # 激活新环境new
  source activate new
  
  # 在新环境下安装ipykernel
  conda install ipykernel
  
  # 将new写入notebook
  python -m ipykernel install --user --name your_env_name --display-name your_env_name
  ```

## pip

* 从txt安装包 `pip install -r requirements.txt`

## 语法

* python添加目录环境

  ```python
  import os
  import sys
  
  SQ_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  sys.path.append(SQ_DIR)
  ```
  
  
  
* 执行cmd

  ```python
  import os
  cmd = 'touch a.txt'
  os.system(cmd)
  ```
  
* try  except

  ```python
    import traceback
    try:
        2/0
    except Exception as e:
        # traceback.print_exc()    # 等同于  print( traceback.format_exc() )
        # traceback.format_exc()  # 返回字符串，供日志文档使用
        logging.error( traceback.format_exc() )
  ```

* 使用显卡

  ```python
  import os
  os.environ["CUDA_VISIBLE_DEVICES"] = "1"
  
  或者
  CUDA_VISIBLE_DEVICES="1" python xx.py
  ```

* 常用函数

  * 遍历文件夹下文件

    ```Python
    def findAllFile(base):
        for root, ds, fs in os.walk(base):
            for f in fs:
                fullname = os.path.join(root, f)
                yield fullname
                
    # 调用：
    base = '/working/financial_news_insight_system/data/history_data/data1'
    for i in findAllFile(base):
        print(i)
    ```
    
  * generator合并
  
    ```python
    from itertools import chain
    def gen1():
        for item in 'abcdef':
            yield item
    
    def gen2():
        for item in '123456':
            yield item
            
    gen = chain(gen1(), gen2())
    for i in gen:
        print(i)
    ```

## 常用三方库

### pandas

* 12种Numpy&Pandas高效技巧 [wechat](https://mp.weixin.qq.com/s/HBwRtnVvjPhRyDyJJkenyQ)

* 

### 其他

* 调试 icecream https://mp.weixin.qq.com/s/nChgqNcXla7_RTpcgOMhoA

  ```python
  pip install icecream
  
  # 使用
  from icecream import ic 
  def plus_five(num):
      return num + 5
  ic(plus_five(4))
  ```
  
* 输出表格 prettytable  https://mp.weixin.qq.com/s/nFNCCUfgg1lSnCFMlpLaBw

  ```python
  pip install prettytable
  
  import sys
  from prettytable import PrettyTable
  reload(sys)
  sys.setdefaultencoding('utf8')
  
  table = PrettyTable(['编号','云编号','名称','IP地址'])
  table.add_row(['1','server01','服务器01','172.16.0.1'])
  table.add_row(['2','server02','服务器02','172.16.0.2'])
  ```

  

# LINUX

```
# cmd下ssh直连
ssh dejian@49.52.10.198
```



## cuda

* 查看cuda版本

  ``````python
  # 查看cuda版本
  cat /usr/local/cuda/version.txt
  
  # 查看cudnn版本
  cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
  ``````

* 查看显卡型号 
  
  * `lspci | grep VGA` 或者 `lspci | grep NVIDIA`

## shell

* [基础语法](https://zhuanlan.zhihu.com/p/102176365)

## 系统

* 重启、关机

  ```
  # 重启
  shutdown -r now
  
  
  # 关机
  shutdown -h now 立刻关机
  ```

  

* 创建软连接

  ```
  ln -s 源文件(实际存储文件的地方) 目标文件（）
  ```

* 查看文件夹大小

  ```python
  du --max-depth 1 -lh  该文件夹的完整路径
  ```

* 查看统计当前文件夹下文件数量（不包括目录）

  ```
  ls -l | grep "^-" | wc -l
  ```
  
  

* 查看系统信息 [详细参考](https://blog.csdn.net/qq_31278903/article/details/83146031)

  ```python
  uname －a   （Linux查看版本当前操作系统内核信息）
  cat /proc/version （Linux查看当前操作系统版本信息）
  cat /etc/issue  或cat /etc/redhat-release（Linux查看版本当前操作系统发行版信息）
  ```

* 查看端口
  
  * `lsof -i:端口号`
  
* 清理显存
    ```python
    # 这时候可以使用如下命令查看到top或者ps中看不到的进程，之后再kill掉：
    fuser -v /dev/nvidia*
    
    # 接着杀掉显示出的进程（有多个）
    kill -9 xxxx
    ```

* 清空缓存

  ```python
  $ sync
  $ echo 3 >/proc/sys/vm/drop_caches 
  # 不行的话：
  $ sudo sh -c "echo 3 > /proc/sys/vm/drop_caches"
  ```

* 创建用户、组

  ```
  直接 sudo adduser xxx   # 是adduser就不会有下面这些问题了
  ----------------------------------------------------------
  创建用户               sudo useradd xxx
  更改用户的密码     	sudo passwd xxx
  更改用户组       	  sudo chgrp jianxiang jianxiang
  更改目录所有者     	sudo chown -R jiawei /home/jiawei
  更改用户权限        	 sudo chmod 755 /home/jianxiang
  更改bash:             chsh -s /bin/bash
  ```

* 删除用户所有进程

    ```Shell
    killall -u tian
    ```

    

* centos 安装包
    ```powershell
    # 先运行下面
    yum install -y epel-release
    # 再运行
    yum install 包名字
    ```

* 远程拷贝

  ```python
  scp test.txt dejian@49.51.10.198:/home/dejian/.bashrc
  ```

* 定时任务：crontab https://www.runoob.com/linux/linux-comm-crontab.html

## docker

* docker重启

  ```python
  # 启动
  systemctl start docker
  # 停止
  systemctl stop docker
  # 重启
  systemctl restart docker
  
  ```
  
  
  
* 镜像迁移

  ```python
  # 将镜像保存为压缩包文件
  docker save  jingxiangname | gzip > xxxx.tar.gz
  
  # 加载镜像
  docker load -i xxxx.tar.gz
  ```

* 创建容器

  ```
  docker run --gpus all --name financial --shm-size 8G -p 9222:9222 -p 9333:9333 -p 9223:9223 -v /home/working:/working -it -d hunter:v3
  ```

* 查看容器挂载目录

  ```python
  docker inspect container_id | grep Mounts -A 20
  ```

* 删除

  ```python
  # 删除容器
  docker rm 容器ID
  
  # 删除镜像
  docker rmi 容器名称:version
  docker rmi 容器ID
  ```

* 配置ssh, 用vscode连接

  * ![image-20210304124758978](https://i.loli.net/2021/03/04/NDpYwryEt3Im8oW.png)

  * ```python
    docker exec -it container_name /bin/bash
    apt install -y openssh-server
    
    # 以下是配置自己的docker容器内的ssh服务，只需要替换new_passwd为自己的密码即可
    mkdir /var/run/sshd
    echo "root:new_passwd" | chpasswd
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
    ehco "export VISIBLE=now" >> /etc/profile
    
    service ssh restart
    ```

    

## java

* 开源jdk安装

  ```python
  sudo apt-get update	#更新软件包列表
  sudo apt-get install openjdk-8-jdk	# 安装openjdk-8-jdk
  java -version	# 查看是否安装成功
  ```

## kafka

* 安装教程[链接](https://blog.csdn.net/msllws/article/details/106615536)

# Pytorch

* tensor numpy list互转

```Python
# 1.1 list 转 numpy
ndarray = np.array(list)

# 1.2 numpy 转 list
list = ndarray.tolist()

# 2.1 list 转 torch.Tensor
tensor=torch.Tensor(list)

# 2.2 torch.Tensor 转 list
# 先转numpy，后转list
list = tensor.numpy().tolist()

# 3.1 torch.Tensor 转 numpy
ndarray = tensor.numpy()
# *gpu上的tensor不能直接转为numpy
ndarray = tensor.cpu().numpy()

# 3.2 numpy 转 torch.Tensor
tensor = torch.from_numpy(ndarray) 
```

* PyTorch Cookbook 常用代码合集 https://mp.weixin.qq.com/s/7at6y2NcYaxGGN8syxlccA

# 工具

## git 

* 微信总结 https://mp.weixin.qq.com/s/Z0wyl90ZAXPOSm7c8_jvug

* 设置当前分支为默认提交分支

    ```python
    git branch --set-upstream-to=origin/master master
    ```

    

* 提交状态

    ![image-20210228184811286](https://i.loli.net/2021/02/28/EuPXVpIbUK8SxQr.png)

* 生成ssh秘钥

  ```python
  ssh-keygen -t rsa -C "你的邮箱"
  ```

* 配置远程git（不重复输密码）

    ```python
    # 删除远程地址
    git remote rm origin
    
    # 配置
    git remote add origin http://账号:密码@precision:9190/financial-news-insight/financial_news_insight_system.git
    ```

    

* git add

  * ![image-20201110151107073](https://i.loli.net/2020/11/10/OsdrI6ZfY3HQqte.png)

* git stash

  ```shell
  git stash [save message]	保存，save为可选项，message为本次保存的注释
  git stash list				所有保存的记录列表
  git stash pop stash@{num}	恢复，num是可选项，通过git stash list可查看具体值。只能恢复一次
  
  
  git stash drop stash@{num}  丢弃stash@{$num}存储，从列表中删除这个存储
  git stash clear   删除所有缓存的stash
  ```

* git拉取远程强制覆盖本地代码
    ```
    git fetch --all 
    git reset --hard origin/master 
    git pull
    ```

* git 信息中 中文显示ascii码（乱码）：

  ```python
  git config --global core.quotepath false
  git config --global i18n.commit.encoding utf-8
  git config --global i18n.logoutputencoding utf-8
  export LESSCHARSET=utf-8
  ```

  

## tmux

* 创建 `tmux new -s <session-name>`

* 接入 `tmux attach -t <session-name>`

* 删除  `tmux kill-session -t <session-name>`

* 查看 `tmux ls`

* 功能键 ctrl+b
  * 分离 ：功能键、d
  * 左右新窗口 ：功能键、%
  * 关闭窗口：功能键 、x
  
  
## vim
 * 解决复制串行
    ```
    复制前：
    set paste
    
    复制后：
    set nopaste
    ```

## markdown

* 添加目录： [toc] 

## 查询ip

* 查询 ip:  https://www.ipaddress.com/

## 定时任务crontab

* 安装 apt-get install cron
* 查看是否运行 service cron status
* 启动   sudo  service cron start
* 查看日志 tail -f /var/log/cron.log
* 菜鸟教程 https://www.runoob.com/linux/linux-comm-crontab.html
* 相关问题 https://blog.csdn.net/u011734144/article/details/54576469

## apps

* 标注工具
  * doccano https://github.com/doccano/doccano