---
title: "Установить Hugo web сервер на Ubuntu 22"
date: 2023-12-19T04:11:25Z
draft: true

categories: ["Hugo"]
tags: []
toc: false
author: ""
---
# Установить Hugo на linux:
```bash
sudo apt install hugo
$ hugo version
hugo v0.92.2+extended linux/amd64 BuildDate=2023-01-31T11:11:57Z VendorInfo=ubuntu:0.92.2-1ubuntu0.1

hugo new site git4mind
Congratulations! Your new Hugo site is created in /home/administrator/sites/git4mind.
...
~/sites$ ll git4mind/

```

![[Создаем статичный сайт на Hugo#каталоги hugo]]

```bash
cd git4mind/
pwd
/home/administrator/sites/git4mind

git init
```

Устанавим тему bilberry-hugo-theme[^1]:

```bash
git submodule add https://github.com/uPagge/bilberry-hugo-theme/ themes/bilberry-hugo-theme

Cloning into '/home/administrator/sites/git4mind/themes/bilberry-hugo-theme'...
remote: Enumerating objects: 4309, done.
remote: Total 4309 (delta 0), reused 0 (delta 0), pack-reused 4309
Receiving objects: 100% (4309/4309), 18.99 MiB | 553.00 KiB/s, done.
Resolving deltas: 100% (2531/2531), done.

ll themes/bilberry-hugo-theme/
total 23688
...
```

- [ ] **?** `git submodule add` - особая ветка git?

```bash
# гипотеза: github диалект git 
# вложенный репозиторий в themes/bilberry-hugo-theme/
-rw-rw-r--  1 administrator administrator      54 дек 19 01:49 .git
drwxrwxr-x  3 administrator administrator    4096 дек 19 01:49 .github/

```

Включаем в git-отслеживание папки:
```bash
git add archetypes
git add config.toml
git add content
git add data
git add layouts
git add static
git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .gitmodules
        new file:   archetypes/default.md
        new file:   config.toml
        new file:   themes/bilberry-hugo-theme

```

Настройка темы[^2]

```shell
cp themes/bilberry-hugo-theme/exampleSite/config.toml . 
cp -r themes/bilberry-hugo-theme/archetypes/ .

# запуск веб сервера
hugo server -D
Start building sites …
hugo v0.92.2+extended linux/amd64 BuildDate=2023-01-31T11:11:57Z VendorInfo=ubuntu:0.92.2-1ubuntu0.1

                   | EN | DE
-------------------+----+-----
  Pages            | 13 | 12
  Paginator pages  |  0 |  0
  Non-page files   |  0 |  0
  Static files     | 64 | 64
  Processed images |  0 |  0
  Aliases          |  2 |  1
  Sitemaps         |  2 |  1
  Cleaned          |  0 |  0

...

Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)

```

###### настройки nginx
```bash
user administrator;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
}
http {
		server {
                location / {
                        proxy_pass http://localhost:1313;
                }
        }
}
```

![[создать hugo сайт.png]]


Добавляем контент[^3]:
```bash
$ hugo new article/first_post.md
Content "/home/administrator/sites/git4mind/content/article/first_post.md" created

```

[^1]: [[Создаем статичный сайт на Hugo#Устанавить тему bilberry-hugo-theme]]
[^2]: [[Создаем статичный сайт на Hugo#Настройка темы]]
[^3]: [[Создаем статичный сайт на Hugo#Добавляем контент]]
