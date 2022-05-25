# github的contribution显示问题

`ssh-keygen -t rsa -C "邮箱"` 这里生成密钥时指定的邮箱其实无所谓是什么，只是起作标识的作用。

但是，本地`git config`所设置的`user.name`和`user.email`则只有两者和远程仓库`github`上登录的一样，`github`上的账号的仓库才能显示`contribution`.

## git config示例

```
git config user.name "这里换上你的用户名"
git config user.email "这里换上你的邮箱"
```

git config有几个类型：

* `git config --system`：存放在`/etc/gitconfig`，针对整个系统生效的，几乎不会使用
* `git config --global`：存放在`~/.gitconfig`，这个是针对用户的，对系统中这个用户的所有项目都生效，很常用
* `git config --local`：存放在在项目的`.git/config`，这是针对某个项目设置用户名和邮箱的（也是不加选项时默认的选项）

还有其它选项，可输入`git config`查看。