# Konfigurisanje git servera

### Debian instalacija
```
apt install git gitweb -y
```

## Na serveru
```
cd /

mkdir repo && cd repo

git init

echo "# Readme" > README.md

git add -A

git commit -m "Initial Commit"
```

## Na računaru klijenta
```
git clone ssh://root@server_ip/repo

# ili sa brojem porta:

git clone ssh://root@server_ip:22/repo
```

> NAPOMENA: Putanja definisana u url adresi za kloniranje predstavlja unutrašnju putanju servera.

#### Kada je repozitorija klonirana:
```
echo "# README" >> readme.txt

git add -A

git commit -m "First push"

git push -u origin master
```

### Gitweb