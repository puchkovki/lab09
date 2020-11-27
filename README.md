## Laboratory work IX [![Build Status](https://travis-ci.com/puchkovki/lab09-Github-Release.svg?branch=master)](https://travis-ci.com/puchkovki/lab09-Github-Release)

<a href="https://yandex.ru/efir/?stream_id=vYrKRcFKi46o"><img src="https://raw.githubusercontent.com/tp-labs/lab09/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению процесса создания артефактов на примере **Github Release**

```sh
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_TOKEN=<полученный_токен>
$ export GITHUB_USERNAME=<имя_пользователя>
$ export PACKAGE_MANAGER=<пакетный менеджер>
$ export GPG_PACKAGE_NAME=gpg
```

```sh
# for *-nix system
$ $PACKAGE_MANAGER install xclip
$ alias gsed=sed
$ alias pbcopy='xclip -selection clipboard'
$ alias pbpaste='xclip -selection clipboard -o'
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
~/Documents/Github/puchkovki/workspace ~/Documents/Github ~ ~/Documents/Github/puchkovki
$ source scripts/activate
$ go get github.com/aktau/github-release # Established command line utility to make releases and attach artefacts to them
```

```sh
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09
Клонирование в «projects/lab09»…
Распаковка объектов: 100% (72/72), готово.
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09
```

```sh
$ gsed -i 's/lab08/lab09/g' README.md
```

```sh
$ $PACKAGE_MANAGER install ${GPG_PACKAGE_NAME} # Installed gpg package
Уже установлен пакет gpg самой новой версии (2.2.12-1ubuntu3).
gpg помечен как установленный вручную.
$ gpg --list-secret-keys --keyid-format LONG # Listed all gpg keys
$ gpg --full-generate-key # Generated new key
Выберите тип ключа:
   (1) RSA и RSA (по умолчанию)
   (2) DSA и Elgamal
   (3) DSA (только для подписи)
   (4) RSA (только для подписи)
Ваш выбор? 1
длина ключей RSA может быть от 1024 до 4096.
Какой размер ключа Вам необходим? (3072) 4096
Запрошенный размер ключа - 4096 бит
Выберите срок действия ключа.
         0 = не ограничен
      <n>  = срок действия ключа - n дней
      <n>w = срок действия ключа - n недель
      <n>m = срок действия ключа - n месяцев
      <n>y = срок действия ключа - n лет
Срок действия ключа? (0) 0
Срок действия ключа не ограничен
Все верно? (y/N) y

GnuPG должен составить идентификатор пользователя для идентификации ключа.

Ваше полное имя: Kyryll Puchkov
Адрес электронной почты: puchkov.k@phystech.edu
Примечание: github release
Вы выбрали следующий идентификатор пользователя:
    "Kyryll Puchkov (github release) <puchkov.k@phystech.edu>"

Необходимо получить много случайных чисел. Желательно, чтобы Вы
в процессе генерации выполняли какие-то другие действия (печать
на клавиатуре, движения мыши, обращения к дискам); это даст генератору
случайных чисел больше возможностей получить достаточное количество энтропии.
gpg: ключ 64620FD52C25DEC4 помечен как абсолютно доверенный
gpg: создан каталог '/home/puchkovki/.gnupg/openpgp-revocs.d'
gpg: сертификат отзыва записан в '/home/puchkovki/.gnupg/openpgp-revocs.d/25A64DA9896A767F71A34C7964620FD52C25DEC4.rev'.
открытый и секретный ключи созданы и подписаны.

pub   rsa4096 2020-04-13 [SC]
      
uid                      Kyryll Puchkov (github release) <puchkov.k@phystech.edu>
sub   rsa4096 2020-04-13 [E]


$ gpg --list-secret-keys --keyid-format LONG
gpg: проверка таблицы доверия
gpg: marginals needed: 3  completes needed: 1  trust model: pgp
gpg: глубина: 0  достоверных:   1  подписанных:   0  доверие: 0-, 0q, 0n, 0m, 0f, 1u
/home/puchkovki/.gnupg/pubring.kbx
----------------------------------
sec   rsa4096/ 2020-04-13 [SC]
uid               [  абсолютно ] Kyryll Puchkov (github release) <puchkov.k@phystech.edu>
ssb   rsa4096/ 2020-04-13 [E]
$ gpg -K ${GITHUB_USERNAME} # Listed all secret keys for user ${GITHUB_USERNAME}
$ GPG_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep ssb | tail -1 | awk '{print $2}' | awk -F'/' '{print $2}')  # Listed the ssb key of last key and got the second element from the second column
$ GPG_SEC_KEY_ID=$(gpg --list-secret-keys --keyid-format LONG | grep sec | tail -1 | awk '{print $2}' | awk -F'/' '{print $2}') # Listed the sec key of last key and got the second element from the second column
$ gpg --armor --export ${GPG_KEY_ID} | pbcopy  # Exported key id through into the clipboard
$ pbpaste # Imported from the clipboard
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBF6UjJABEADb35+OjGpnLLBySroIv5XWnyd0B9CtfPLhKBc3m1KtfDhWYt7X
...
1vwsOnDoN/YTomih0Gp/P+vnEoU=
=hYY1
-----END PGP PUBLIC KEY BLOCK-----
$ open https://github.com/settings/keys # Added gpg github key in github keys settings from clipboard
$ git config user.signingkey ${GPG_SEC_KEY_ID} # Added ${GPG_SEC_KEY_ID} as signing key locally in user section 
$ git config gpg.program gpg # Use gpg package in case of errors
```

```sh
$ test -r ~/.bash_profile && echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile # Added new GPG_TTY terminal and switched to the out bash
$ echo 'export GPG_TTY=$(tty)' >> ~/.profile
```

```sh
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
[ 50%] Built target gtest
[100%] Built target gtest_main
loading initial cache file /home/puchkovki/.hunter/_Base/5659b15/810fbc6/9775f4e/Build/GTest/args.cmake
[100%] Completed 'GTest-Debug'
[100%] Built target GTest-Debug
-- [hunter] Build step successful (dir: /home/puchkovki/.hunter/_Base/5659b15/810fbc6/9775f4e/Build/GTest)
-- [hunter] Cache saved: /home/puchkovki/.hunter/_Base/Cache/raw/8966534d0796089e12733c9d19c8dc65c12d764c.tar.bz2
-- Configuring done
-- Generating done
-- Build files have been written to: /home/puchkovki/Documents/Github/puchkovki/workspace/projects/lab09/_build
$ cmake --build _build --target package
[ 25%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[ 50%] Linking CXX static library libprint.a
[ 50%] Built target print
[ 75%] Building CXX object CMakeFiles/demo.dir/demo/main.cpp.o
[100%] Linking CXX executable demo
[100%] Built target demo
CPack: Create package using TGZ
CPack: Install projects
CPack: - Run preinstall target for: print
CPack: - Install project: print
CPack: Create package
CPack: - package: /home/puchkovki/Documents/Github/puchkovki/workspace/projects/lab09/_build/print-0.1.0.0-Linux.tar.gz generated.
```

```sh
$ travis login --auto
Username: puchkovki
Password for puchkovki: *************
Successfully logged in as puchkovki!
$ travis enable
Detected repository as puchkovki/lab09, is this correct? |yes| yes
puchkovki/lab09: enabled :)
```

```sh
$ git tag -s v0.1.0.0 # Added tag with digital signature
$ git tag -v v0.1.0.0 # Showed information about v0.1.0.0 tag
object 7679a270d70eb336d5d1b8b066826650a3964f3f
type commit
tag v0.1.0.0
tagger puchkovki <puchkov.k@phystech.edu> 1586797485 +0300

0.1.0.0 release
gpg: Подпись сделана Пн 13 апр 2020 20:05:15 MSK
gpg:                ключом RSA с идентификатором ...
gpg: Действительная подпись пользователя "Kyryll Puchkov (github release) <puchkov.k@phystech.edu>" [абсолютное]
$ git show v0.1.0.0 # Showed detailed information about v0.1.0.0 tag
tag v0.1.0.0
Tagger: puchkovki <puchkov.k@phystech.edu>
Date:   Mon Apr 13 20:04:45 2020 +0300

0.1.0.0 release
-----BEGIN PGP SIGNATURE-----

iQIzBAABCgAdFiEEJaZNqYlqdn9xo0x5ZGIP1Swl3sQFAl6Um8sACgkQZGIP1Swl
...
=u+AG
-----END PGP SIGNATURE-----

commit 7679a270d70eb336d5d1b8b066826650a3964f3f (HEAD -> master, tag: v0.1.0.0, origin/master)
Author: puchkovki <puchkov.k@phystech.edu>
Date:   Thu Apr 9 21:44:35 2020 +0300

    adding Dockerfile

diff --git a/.travis.yml b/.travis.yml
index 4d6e5c6..c30bb1b 100644
--- a/.travis.yml
+++ b/.travis.yml
@@ -1,15 +1,8 @@
-language: cpp
+language: generic
+
+services:
+- docker
 
 script:
-- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON
-- cmake --build _build
-- cmake --build _build --target install
$ git push origin master --tags  # Pushed tags
Всего 73 (изменения 17), повторно использовано 0 (изменения 0)
remote: Resolving deltas: 100% (17/17), done.
To https://github.com/puchkovki/lab09.git
 * [new branch]      master -> master
 * [new tag]         v0.1.0.0 -> v0.1.0.0
```

```sh
$ github-release --version # Showed github release version
github-release v0.7.2
$ github-release info -u ${GITHUB_USERNAME} -r lab09 # Listed tags and releases
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/puchkovki/lab09/commits/7679a270d70eb336d5d1b8b066826650a3964f3f)
releases:
$ github-release release \ # Added new libprint release for puchkovki user in lab09 repo from v0.1.0.0 tag with description
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```

```sh
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m` # Established OS name and architecture
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz 
$ github-release upload \ # Added tar.gz file to release
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
```

```sh
$ github-release info -u ${GITHUB_USERNAME} -r lab09
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/puchkovki/lab09/commits/7679a270d70eb336d5d1b8b066826650a3964f3f)
releases:
- v0.1.0.0, name: 'libprint', description: 'my first release', id: 25474762, tagged: 13/04/2020 at 17:04, published: 14/04/2020 at 07:30, draft: ✗, prerelease: ✗
  - artifact: print-Linux-x86_64.tar.gz, downloads: 0, state: uploaded, type: application/octet-stream, size: 6.6 kB, id: 19757330
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME} # Downloaded the artifact
--2020-04-14 10:47:03--  https://github.com/puchkovki/lab09/releases/download/v0.1.0.0/print-Linux-x86_64.tar.gz
Сохранение в: «print-Linux-x86_64.tar.gz»

print-Linux-x86_64.tar. 100%[=============================>]   6,45K  --.-KB/s    за 0,05s   

2020-04-14 10:47:05 (131 KB/s) - «print-Linux-x86_64.tar.gz» сохранён [6601/6601]
$ tar -ztf ${PACKAGE_FILENAME}
```

## Report

```sh
$ popd
$ export LAB_NUMBER=09
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2015-2020 The ISC Authors
```
