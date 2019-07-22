# Python全般

## Python.hの在り処

バージョン
Linux raspberrypi 4.19.42-v7+ #1219 SMP Tue May 14 21:20:58 BST 2019 armv7l GNU/Linux

- /usr/include/python2.7/Python.h
- /usr/include/python3.5m/Python.h

このあたり。

```g++ -I/usr/include/python3.5m soruce.cpp -o out```

という感じで使えばいい。

