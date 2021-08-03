# docker-compose_args
docker-compose起動時にentrypointに引数を渡す

## ファイル内容

### start.sh

引数の値を出力する。
``` sh
#!/bin/bash

echo 'Hello' $1 '!!'
```

### Dockerfile

ある意味なんでもいいといえば何でもいい。
``` Dockerfile
FROM ubuntu

WORKDIR /

COPY start.sh /start.sh
RUN chmod 755 /start.sh

# ここでは引数を指定しない。docker-composeコマンドで指定する。
ENTRYPOINT [ "/start.sh" ]

```

### docker-compose.yml

``` yml
version: '3'
services:
  shell:    # この値（shell）を起動時に使う。
    build: .
    container_name: container_shell
    volumes:
      - ./start.sh:/start.sh

```

## 実行コマンド

``` sh
# docker-compose upではなく・・・
docker-compose run ${サービス名} ${引数}
```

### 実際に実行してみる

``` sh
docker-compose run shell hoge
```

実行結果

``` sh
Creating docker-compose_args_shell_run ... done
Hello hoge !!
```

## 参考

- [](https://www.it-swarm-ja.com/ja/docker-compose/dockercomposeyml%E3%81%A7entrypoint%E3%81%AB%E5%BC%95%E6%95%B0%E3%82%92%E6%B8%A1%E3%81%99%E6%96%B9%E6%B3%95/826203749/)