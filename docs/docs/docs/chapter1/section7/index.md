# 修改本地仓库和远程仓库的名字

## 1 修改远程仓库的名字

在github上找到settings，直接rename

## 2 修改本地仓库的名字

直接重命名本地仓库所在的目录的名字。

然后，重新关联远程仓库与本地仓库。

```
git remote rm origin
git remote add origin git@github.com:qgao233/qgaoFinancialPortfolio.git
git push -u origin master
```