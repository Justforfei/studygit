$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

cd d:/  切换到D盘

创建一个空目录：
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit

pwd命令用于显示当前目录

确保目录名（包括父目录）不包含中文

通过git init命令把这个目录变成Git可以管理的仓库：
会创建一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件

注：所有的版本控制系统，其实只能跟踪文本文件的改动，不要用window自带的文本编辑器

$ git add readme.txt

$ git commit -m "wrote a readme file"
-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件



//总结
--global参数，表示你这台机器上所有的Git仓库都会使用这个配置
初始化一个Git仓库，使用git init命令。
添加文件到Git仓库，分两步：
第一步，使用命令git add <file> ，注意，可反复多次使用，添加多个文件；
第二步，使用命令git commit，完成。

git status命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个“distributed”单词。

//总结
要随时掌握工作区的状态，使用git status命令。
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看
git log命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是“append GPL”，上一次是“add distributed”，最早的一次是“wrote a readme file”。 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 --pretty=oneline参数：

你看到的一大串类似3628164...882e1e0的是commit id（版本号）

Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

我们要把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用git reset命令：

最新的那个版本“append GPL”已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个“append GPL”的commit id是3628164...，于是就可以指定回到未来的某个版本：

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向“append GPL”：

git-head

改为指向“add distributed”：

git-head-move

然后顺便把工作区的文件更新了。所以你让HEAD指向哪个版本号，你就把当前版本定位在哪。

Git提供了一个命令git reflog用来记录你的每一次命令：

//总结
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
git log命令查看文件更新历史记录//$ git log --pretty=oneline
git reflog用来记录你的每一次命令
$ git reset --hard 3628164 回到某个版本
$ git reset --hard HEAD^^ 回退到上上个版本


Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。
工作区（Working Directory）：就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：
版本库（Repository）：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。