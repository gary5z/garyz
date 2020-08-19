
## 下载
```bash
# wget http://download.redis.io/releases/redis-5.0.4.tar.gz
```

## 解压
```bash
# zxvf redis-5.0.4.tar.gz
# mv redis-5.0.4 redis
```

## 编译
```bash
[root@TranslationServer redis]# make
cd src && make all
make[1]: Entering directory `/alidata/server/redis/src'
    CC Makefile.dep
make[1]: Leaving directory `/alidata/server/redis/src'
make[1]: Entering directory `/alidata/server/redis/src'
rm -rf redis-server redis-sentinel redis-cli redis-benchmark redis-check-rdb redis-check-aof *.o *.gcda *.gcno *.gcov redis.info lcov-html Makefile.dep dict-benchmark
(cd ../deps && make distclean)
make[2]: Entering directory `/alidata/server/redis/deps'
(cd hiredis && make clean) > /dev/null || true
(cd linenoise && make clean) > /dev/null || true
(cd lua && make clean) > /dev/null || true
(cd jemalloc && [ -f Makefile ] && make distclean) > /dev/null || true
(rm -f .make-*)
make[2]: Leaving directory `/alidata/server/redis/deps'
(rm -f .make-*)
echo STD=-std=c99 -pedantic -DREDIS_STATIC='' >> .make-settings
echo WARN=-Wall -W -Wno-missing-field-initializers >> .make-settings
.......

Hint: It's a good idea to run 'make test' ;)

make[1]: Leaving directory `/alidata/server/redis/src'
```

## 查看src
可以看到在src目录下生成了几个新的文件
```bash
[root@TranslationServer redis]# ll -tr src

-rwxr-xr-x 1 root root 8101264 May  6 15:28 redis-server
-rwxr-xr-x 1 root root 8101264 May  6 15:28 redis-sentinel
-rw-r--r-- 1 root root  928560 May  6 15:28 redis-cli.o
-rwxr-xr-x 1 root root 4806824 May  6 15:28 redis-cli
-rw-r--r-- 1 root root  109128 May  6 15:28 redis-benchmark.o
-rwxr-xr-x 1 root root 4366592 May  6 15:28 redis-benchmark
-rwxr-xr-x 1 root root 8101264 May  6 15:28 redis-check-rdb
-rwxr-xr-x 1 root root 8101264 May  6 15:28 redis-check-aof
```

## 安装
```bash
[root@TranslationServer redis]# make install
cd src && make install
make[1]: Entering directory `/alidata/server/redis/src'
    CC Makefile.dep
make[1]: Leaving directory `/alidata/server/redis/src'
make[1]: Entering directory `/alidata/server/redis/src'

Hint: It's a good idea to run 'make test' ;)

    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
make[1]: Leaving directory `/alidata/server/redis/src'
```

