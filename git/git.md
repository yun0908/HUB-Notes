# git
## 一、常用命令
- git config --global user.name 用户名          设置用户签名
- git config --global user.email 邮箱              设置用户签名
- git init                                                           初始化本地库
- git status                                                       查看本地库状态
- git add 文件名                                               添加到缓存区
- git commit -m "日志信息“  文件名                提交到本地库
- git reflog                                                        查看历史记录
- git reset --hard 版本号                                   版本穿梭


## 二、流程
### 1、初始化本地库
#### git init
![[Pasted image 20250126120323.png]]
### 2、暂存区
- git add 文件名                                    添加到缓存区
- git rm --cached 文件名                       从缓存区删除

### 3、本地库
- git commit -m “日志信息” 文件名      将暂存区文件提交到本地库
- git reflog                                             查看版本
- git flog                                                查看详细日志
### 4、历史版本和版本穿梭
- git reflog                                             查看版本信息
- git log                                                  查看版本详细信息
- git reset --hard 版本号                        版本穿梭
### 5、分支操作
 - 同时并行推进多个功能开发，提高开发效率。
 - 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。

- git branch 分支名                                创建分支
- git branch -v                                        查看分支
- git checkout 分支名                             切换分支
- git merge 分支名 ~~被合并的分支名~~       把指定的分支合并到当前分支上

##### 合并冲突
- 合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改，Git无法替我们决定使用哪一个，必须人为决定新代码内容
###### 解决冲突
- 1) 编辑有冲突的文件，删除特殊符号，决定要使用的内容
		特殊符号：<<<<<HEAD  当前分支的代码  ===== 合并过来的代码 >>>>hot-fix
- 2) 添加到缓存区
		git add 文件名
- 3) 执行提交（注意：此时使用 git commit 命令 不能带文件名）
### 6、Github操作
- 1) 创建远程仓库
			
- 