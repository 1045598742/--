# git

~~~js
cd d://切换目标路径
rm index.txt//删除 index.txt文件
rm -rf index //删除index文件夹（包括里面的所有东西，递归）
rm -rf * //删除所有（谨慎使用）
touch 1.txt //创建1.txt文件   
mkdir hehe //创建一个叫hehe的文件夹
cat 1.txt//查看1.txt里面的内容
vi 1.txt //编辑内容第一步 然后 按 i键进行编辑模式 然后esc退出编辑模式 :键继续操作 如果这时候不想改了后面加q！回车强制推出这时候 ，wq键保存后退出
ls -al//查看所有文件列表（.git）
git add -A或者.  //添加所有文件到暂存区 多个文件就一个一个文件名去写(工作区是红色，暂存区是绿色，历史区是无色)
git rm --cached 1.txt//取消暂存区的1.txt文件
git rm --cached . -r //取消所有暂存区文件(-r是必须的，不写不让删除)
git commit -m'提交的备注消息' //进行代码提交
git diff//比较各个区的代码不同（默认是工作区和暂存区比较）
git diff '分支名' //工作区和历史区比较代码
git diff --cached //暂存区和历史区比较
git checkout ./文件名 //工作区的代码从暂存区回退到工作区这次的修改）
git reset HEAD ./文件名 //暂存区代码回退一次 然后git checkout ./文件名 再回退到工作区（这种情况适用于已经添加到暂存区了，再想回滚代码的情况）
history >历史记录.txt //生成一个文件历史记录操作文件
echo hello > 1.txt //创建一个1.txt文件并写入 hello
echo hello >> 1.txt //在1.txt文件追加写入 hello
git commit -am '提交备注' //将工作区代码提交到暂存区再提交到历史区(简便操作,前提是这文件已经之前提交过才能用这个简洁命令)
git reset --hard 6707a865d295//历史区代码回滚某一个版本(版本号用7-8位就行，不用全粘贴，注意：回退了版本再打印git log再看版本的话比回滚版本离现在时间更近的那些版本就看不到了，所以要用git reflog，这才是查看所有版本的好办法，所有区的代码都会回滚)
git reset --hard HEAD^ //回滚到上个版本
git branch /查看分支
git branch -v //看有多少分支和每个分支的最后一次版本号和版本备注
git branch 分支名 //创建分支
git checkout 分支名 //切换分支
git branch -D 分支名 //删除某个分支(前提是必须再其他分支下，自己不能删除自己）
git checkout -b 分支名 //创建并切换分支
//如果在dev分支创建了一个文件1.js ，此时切换到master分支也能看到这个文件，即使dev分支git add 1.js之后，master也是一样的状态(还是共享的1.js文件)，只有dev git commit -m'add-1.js'之后，1.js才真正属于dev分支，master就看不到这个文件了
git stash //暂存文件(分支有更改不能直接切换，可以提交更改或者暂存更改，它是将暂存区代码覆盖了工作区，这样就可以切换分支了，如果切回来dev分支，找到刚才暂存的文件，使用 git stash pop 就可以回去刚才未修改完的代码)
git log //普通查看
git reflog //查看所有日志 ，回退了也能查看所有记录（最全的日志）
git log --oneline //查看提交日志简介模式
git log -n2 --oneline  //查看最近两次的提交日志
git merge '分支名' //合并分支(记得在master分支上进行合并，否则主分支就换了)
//注意：如果合并时出了代码冲突，需要手动解决冲突之后再git add然后再git commit
//一定要有根提交才能创建分支
//创建全局账号（本地账号就是--local）
git config --global user.name 'libo'
git config --global user.email '1045598742@qq.com'

//创建某个仓库的账号（local优先级比global高）
git config --global user.name 'libo'
git config --global user.email '1045598742@qq.com'

git init //创建git仓库
cp ../first/index.html //拷贝你想要托管的代码
git add index.html //将代码添加到暂存区
git status //查看当前状态
git commit -m'add index' //进行代码提交
git add img index //多个文件到暂存
//有问题git add -u  //所有文件到暂存区

//如果修改同一文件了（我将index.html改成了index1.html）
git add index1.html
git rm index.html

git resrt --hard //add到暂存区的那些文件回退

git mv index.html index1.html//直接修改文件后添加到暂存区

git branch -v //看有多少分支
git checkout -b temp //创建分支temp
git checkout （分支名） //切换分支

~~~

![1584189821072](D:\学习笔记\img\1584189821072.png)



![1584190362835](D:\学习笔记\img\1584190362835.png)

## 远程仓库

~~~js
git remote add origin https://github.com/1045598742/node_project.git //origin是你自己起的别名，根据你自己喜好，
git remote -v //查看远程仓库状态一般有两个（一个拉取和一个提交）
//如果git remote add b https://**sdsd**dd**.dd(这样错的地址，需要git remote rm b 删除关联)
git push -u origin master //向远程仓库提交代码
git pull origin master//拉取代码
ssh-keygen -t rsa -C 邮箱 //配置公钥和私钥，然后一路回车，结束后查看.ssh文件，cd ~/.ssh然后cat id_rsa.pub，复制密码，然后去github右上角头像里点击setting，找到SSH and GPG keys点进去把密匙粘贴进去确定，之后你再提交代码就不用每次都输入账号和密码了
~~~

~~~js
gh-pages分支来发布我们的静态页
在项目中创建一个gh-pages的分支（分支名固定 不可随意修改）
将分支提到线上仓库
找到提供给你的网站
git checkout -b gh-pages //创建分支并切换
//提交你的文件 然后切换这个分支 取setting找网址
~~~

