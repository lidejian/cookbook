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
  conda create -n ...
  # 查看环境
  conda info --env                               
  conda info -e
  
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

## pip & jupyter

* 从txt安装包 `pip install -r requirements.txt`

* 修改源

  * 临时修改

  ```python
  # -i : 指定源
  # -U : 升级到最新版本
  pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -U funcat
  ```

  * 永久修改 https://mirror.tuna.tsinghua.edu.cn/help/pypi/

  ```python
  pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
  ```

  

* jupyter 设置密码

  ```python
  # 1 浏览器打开jupyter，新建一个python文件，第一行输入：
  from notebook.auth import passwd
  
  # 2 下一行输入：
  passwd()
  回车后提示输入密码，输入两次。
  
  # 哈希密码
  # sha1:158c74966eda:9533217697d0c62f0db43414ec4e06d4072c1cecc2e97d
  
  # 配置
  jupyter notebook --generate-config
  
  ~/.jupyter/jupyter_notebook_config.py
  再次打开配置文件jupyter_notebook_config.py,找到c.NotebookApp.password=第三步的哈希密码，重启jupyter notebook即可
  ```


## 语法

### python添加目录环境

  ```python
  import os
  import sys
  
  SQ_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  sys.path.append(SQ_DIR)
  ```

### 执行cmd

  ```python
  import os
  cmd = 'touch a.txt'
  os.system(cmd)
  ```

### try  except

  ```python
    import traceback
    try:
        2/0
    except Exception as e:
        # traceback.print_exc()    # 等同于  print( traceback.format_exc() )
        # traceback.format_exc()  # 返回字符串，供日志文档使用
        logging.error( traceback.format_exc() )
  ```

### 使用显卡

  ```python
  import os
  os.environ["CUDA_VISIBLE_DEVICES"] = "1"
  
  或者
  CUDA_VISIBLE_DEVICES="1" python xx.py
  ```

### 遍历文件夹下文件

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

### generator合并

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

### 操作excel

```python
# 写excel
import xlwt
# 写excel
workbook = xlwt.Workbook(encoding='utf-8')
worksheet = workbook.add_sheet('Sheet1')

# 写入表头
worksheet.write(0, 0, '标题')
worksheet.write(0, 1, '时间')

worksheet.write(excel_line, 0, news.title) # 写 行、列、内容

# 保存excel
workbook.save(filename.replace('.txt', '.xls'))


# 读excel
import xlrd
book = xlrd.open_workbook('data.xlsx')
sheet1 = book.sheets()[0]
nrows = sheet1.nrows
print('表格总行数',nrows)

ncols = sheet1.ncols
print('表格总列数',ncols)

row3_values = sheet1.row_values(2)
print('第3行值',row3_values)

col3_values = sheet1.col_values(2)
print('第3列值',col3_values)

cell_3_3 = sheet1.cell(2,2).value
print('第3行第3列的单元格的值：',cell_3_3)


#大文件（65536行之外）
import openpyxl
def readExel():
    filename = r'D:\test.xlsx'
    inwb = openpyxl.load_workbook(filename)  # 读文件
    sheetnames = inwb.get_sheet_names()  # 获取读文件中所有的sheet，通过名字的方式
    ws = inwb.get_sheet_by_name(sheetnames[0])  # 获取第一个sheet内容

    # 获取sheet的最大行数和列数
    rows = ws.max_row
    cols = ws.max_column
    for r in range(1,rows):
        for c in range(1,cols):
            print(ws.cell(r,c).value)
        if r==10:
            break

def writeExcel():
    outwb = openpyxl.Workbook()  # 打开一个将写的文件
    outws = outwb.create_sheet(index=0)  # 在将写的文件创建sheet
    for row in range(1,70000):
        for col in range(1,4):
            outws.cell(row, col).value = row*2  # 写文件
        print(row)
    saveExcel = "D:\\test2.xlsx"
    outwb.save(saveExcel)  # 一定要记得保存

```



## 常用三方库

### pandas

* 12种Numpy&Pandas高效技巧 

## numpy

* 手册 https://www.numpy.org.cn/reference/

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
  
* 调试 pysnooper https://github.com/cool-RR/PySnooper

  ```python
  import pysnooper
  
  @pysnooper.snoop()
  def number_to_bits(number):
      if number:
          bits = []
          while number:
              number, remainder = divmod(number, 2)
              bits.insert(0, remainder)
          return bits
      else:
        return [0]
  
  number_to_bits(6)
  # 输出如下
  Starting var:.. number = 6
  15:29:11.327032 call         4 def number_to_bits(number):
  15:29:11.327032 line         5     if number:
  15:29:11.327032 line         6         bits = []
  New var:....... bits = []
  15:29:11.327032 line         7         while number:
  15:29:11.327032 line         8             number, remainder = divmod(number, 2)
  New var:....... remainder = 0
  Modified var:.. number = 3
  ....
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

## 命令

```
less 替换more， 支持上下，PgUp, PgDn

Ctrl + u 清空输入命令
Ctrl + l 清屏

chmod [ugoa] [+-=] [rwx] # user/group/other/all

cp [选项]
	-r 递归整个目录
	-p 保持源文件属性不变
	
	-a 递归整个目录，并将权限也复制过来
	-f 强制覆盖相同文件、目录
	
find [查找范围] [查找条件]
	查找条件：	 -name: 按文件名

cat [-n 输出行号] 显示文件全部内容，将几个文件合并为一个 cat xx.txt xx2.file > file

wc Word Count 统计行号、单词数量
	行数（貌似是从0编号）、单词数、字节

grep [选项] 查找条件 目标文件   ：在文件中搜索匹配的字符并输出
	-i 忽略大小写
	-v 反转查找， 输入与查找不匹配的。
	
	查找条件：
		“^...” 以...开头
		“...$” 以$结尾
		
| 管道，将前一个的输出作为下个的输入，可以无限套娃
	find . -name text.txt | cat -n 	在当前目录下查找，将结果编号显示
	ln -l /etc/ | less		翻页形式查看/etc
	cat /etc/passwd | grep '^sshd' 查找/etc/passwd文件中以sshd开头的行
	ls /user/bin | wc -l  统计/user/bin目录下文件的个数
```

* 重定向

  ```
  输入重定向： 	<
  输出重定向： 	>
  			>> 追加的方式
  错误重定向：	2>	
  			2>>
  输出与错误组合重定向 &>
  ```

* 压缩解压缩

  ```
  .tar
  	解包 tar -svf FileName.tar
  	打包 tar -cvf FileName.tar DirName
  	
  .tar.gz
  	解压 tar -xzvf FileName.tar.gz
  	压缩 tar -czvf FileName.tar.gz DirName
  
  .zip
  	解压 unzip FileName.zip
  	压缩 zip FileName.zip DirName
  
  .rar
  	解压 rar a FileName.rar
  ```

  

## bash/zsh

* 切换

  ```
  # 永久：（需重新登录）
  chsh -s /bin/bash
  chsh -s /bin/zsh
  
  # 临时切换：
  bash
  zsh
  ```

* zsh 插件 https://hufangyun.com/2017/zsh-plugin/

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

## cmd

* 清空DNS缓存 `ipconfig /flushdns`

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
  更改目录所有者     	sudo chown -R jiawei:jiawei /home/jiawei
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
  scp -P 3322 ...  # 添加指定端口
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
  
* 将用户添加到docker组

  ```
  sudo usermod -a -G docker yadong
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

* 从容器创建新镜像

  ```
  docker commit -a "作者" -m "提交信息" a404c6c174a2  mymysql:v1	# a4..为容器名
  ```

* 启动容器并启动相关命令

  ```
  docker exec financial /bin/bash -c "nohup sh /working/start_all.sh > nohup_out.out 2>&1 &"
  ```

* gitlab 安装

  ```
  1 # 官网： https://docs.gitlab.com/omnibus/docker/README.html
  sudo docker run --detach \
    --hostname gitlab.example.com \
    --publish 8443:443 --publish 9190:9190 --publish 2222:2222 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/etc/gitlab:Z \
    --volume /srv/gitlab/logs:/var/log/gitlab:Z \
    --volume /srv/gitlab/data:/var/opt/gitlab:Z \
    gitlab/gitlab-ee:latest			# 指定版本号 不用最新的可以 gitlab/gitlab-ee:11.9.6-ee.0 
  
  2：vi   /srv/gitlab/config/gitlab.rb 修改：
  external_url 'http://precision:9190'
  gitlab_rails['gitlab_shell_ssh_port'] = 2222
   
   
   3 用之前备份恢复  https://blog.csdn.net/foupwang/article/details/94362292
  cd /var/opt/gitlab/backups
  chomd 777 1561597102_2019_06_27_12.0.1_gitlab_backup.tar
  gitlab-ctl stop unicorn
  gitlab-ctl stop sidekiq
  gitlab-rake gitlab:backup:restore BACKUP=备份文件编号
  ```

  

* 删除

  ```python
  # 删除容器
  docker rm 容器ID
  
  # 删除镜像
  docker rmi 容器名称:version
  docker rmi 容器ID
  ```

* docker镜像瘦身  https://github.com/docker-slim/docker-slim

* gitlab 迁移  https://www.cnblogs.com/ssgeek/p/9392104.html

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
    echo "export VISIBLE=now" >> /etc/profile
    
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

* ☆☆☆ 奇淫技巧 https://github.com/521xueweihan/git-tips

* 基本操作：https://labuladong.gitbook.io/algo/di-wu-zhang-ji-shu-wen-zhang-xi-lie/git-chang-yong-ming-ling

* git 设置全局代理

    ```python
    git config --global http.proxy 'socks5://127.0.0.1:7890'
    ```

    

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
* 内网穿透
  
  * https://github.com/ehang-io/nps

## vpn

* 评测
  * https://www.iszy.cc/page/bgfw/
* clash
  * 下载 https://github.com/Fndroid/clash_for_windows_pkg/releases
  * 汉化&便携版下载 https://merlinblog.xyz/wiki/cfw.html
  * 不错的教程 https://merlinblog.xyz/wiki/api.html
  * 免费clash 节点 https://wxf2088.xyz/613.html
  * 免费clash 节点 https://www.butnono.com/latest-2020-freevpn-v2ray-ss-ssr-address.html

# 资源

## 网站

* 将curl保存为代码(python等)  https://curl.trillworks.com/
* 别人笔记
  * https://github.com/gswyhq/hello-world/tree/df78e5fd66ea570f7019da9b1a55a2eda5f2b5d0
  * https://github.com/SmallCao/docutment/tree/d6e6bb22970b8a50f9b22f02297ef572ab970aff
* win10 优化
  * https://www.coolapk.com/feed/24197481?shareKey=YTBiMGI1MWExOTU3NjA0NWUyMDQ~&shareUid=1498994&shareFrom=com.coolapk.market_11.0.2

## 数据

* 中文NLP数据集 https://github.com/InsaneLife/ChineseNLPCorpus
