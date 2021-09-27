# Git push to non-bare repo

()[]

cPanel konfiguracioni fajl:
```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[receive]
        denyCurrentBranch = updateInstead
[gc]
        auto = 0

```

git push to non-bare:
```
git config --local receive.denyCurrentBranch updateInstead
```
<br>
Uspostavljanje radnog procesa:

## Na serveru
```
su git
cd
mkdir demo.git && demo.git
git init
git config --local receive.denyCurrentBranch updateInstead
```

## Na lokalnom računaru
```
mkdir demo && cd demo
git init
git remote add origin ssh://git@123.123.123:/home/demo.git
echo "# Readme" > README.md
git add -A
git commit -m "Readme"
git push -u origin master
```

Ovim se postiže uspostavljanje radnog procesa u kome se sa lokalnog git repozitorijuma izmene šalju u standardni remote repozitorijum, pri čemu se worktree automatski ažurira.