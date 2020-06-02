# git usage

- #### Clone the file from github

```bash
# fisrt method, maybe forget down sub-folder in it.
git clone https://github.com/gdw439/notes

# second method, assure the sub-folder will be download.
git clone --recursive https://github.com/gdw439/notes

# the commed will set git cloning github with sub-folder.
git submodule update --init --recursive  
```

- #### Initlize git

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

- #### update local code

```shell
git commit -m "添加你的注释,一般是一些更改信息"
```

- #### add new file to git

```shell
# .默认将所有文件都添加
git add . 
```

- #### pull code to remote hub

```shell
git pull origin master
# 上传
git push origin master
```

