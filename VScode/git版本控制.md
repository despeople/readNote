```git
初始化：
git init

添加所有文件到暂存区：
git add .

第一次需要配置用户名和邮箱：
git config --global user.email "1649758033@qq.com"
git config --global user.name "despeople"

提交：
git commit -m "你对于这次提交的注释"

查看提交记录：
git log

git创建分支：
git branch feature-branch

git切换分支：
git checkout feature-branch

远程仓库交互：
//首先要在git仓库创建项目
git remote add origin https://github.com/user/repo.git

进行推送：
git push -u origin feature-branch


版本回退：
git历史查看：
git log //查看详细历史记录，按提交时间倒叙排列，包含提交时间，提交作者，提交备注以及提交的hash值；
git log --pretty=oneline //格式化log形式，每条log只有一行，只包含 完整的hash值 和 提交的备注；
git log --oneline //格式化log形式，每条log只有一行，只包含 短hash值 和 提交的备注；

版本回退/切换的命令：
git reset --hard [索引值] //可切换到任意版本[推荐使用这个方式]
git reset --hard HEAD^ //只能后退，一个 ^ 表示回退一个版本，两个^ 表示回退两个版本，。。。依次类推
git reset --hard HEAD~n //只能后退，n表示后退n个版本

```

