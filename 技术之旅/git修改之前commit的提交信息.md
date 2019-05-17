## 1. 修改最新commit的提交信息

```shell
$ git commit --amend
# 进入commit编辑界面，编辑后保存即可
```



## 2. 修改前n次commit的提交信息

```shell
# 假设你需要修改倒数第n次commit的提交信息
$ git rebase -i HEAD~n
# 进入编辑模式，会出现类似以下的内容
pick 6608e22 修改代码结构调整导致不能正常显示的问题
pick 1d381cd 菜单切换可用
...

# 将需要修改的commit的那一行的pick修改为edit，然后保存退出
# 然后输入
$ git commit --amend
# 进入你需要修改的commit编辑界面，编辑后保存退出
# 修改结束后，输入以下命令返回到最新的commit
$ git rebase --continue
```

