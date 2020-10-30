# git常用命令

- #### 克隆工程文件

```bash
# fisrt method, maybe forget down sub-folder in it.
git clone https://github.com/gdw439/notes

# second method, assure the sub-folder will be download.
git clone --recursive https://github.com/gdw439/notes

# the commed will set git cloning github with sub-folder.
git submodule update --init --recursive  
```

- #### 初始化git自己的名字和邮箱

```bash
# 1. 配置自己的git的名字和邮箱
git config --global user.name "your name"
git config --global user.email "email@example.com"

# 2. 创建文件夹， 并在文件夹下面打开git命令行，把这个目录变成git可以管理的仓库
git init

# 3. 可以查看所有文件，包括隐藏文件
ls -a 

# 4. 生成自己的shh key， 然后一路回车
ssh-keygen -t rsa -C "email@example.com"

# 5. 拷贝id_rsa.pub中所有内容，粘贴到github的key中
```

- #### 文件改动和上传

```shell
# 首先， 查看当前工程中的状态，以免忘记保存未保存文件
git status

# 如果需要的话，从远程仓库拉取文件，以保证和远程仓库的同步
git pull origin master

# 然后， 编写代码后，将文件全部添加到暂存区 / 添加单个文件
git add .   / git add fiename.mm

# 接着， 将代码添加备注，介绍自己做了哪方面的修改
git commit -m "改动的内容为xxx"

# 如果需要更新远程仓库， 则需要将其推送到远程仓库
git push origin master
```

- #### 修改远程仓库地址方法

```shell
# 修改命令
git remote set-url origin <url>

# 先删除后添加
git remote rm origin
git remote add origin [url]
```

