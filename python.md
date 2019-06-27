# Python全般

## 現在自分自身を呼び出している「親」を知る

ただしあくまでこれで分かるのは親であって親が呼び出している他のモジュールなどを参照することは出来ない。

```python
import inspect
parent = inspect.getmodule(inspect.stack()[1][0])
```

## Python.hの在り処

バージョン
Linux raspberrypi 4.19.42-v7+ #1219 SMP Tue May 14 21:20:58 BST 2019 armv7l GNU/Linux

- /usr/include/python2.7/Python.h
- /usr/include/python3.5m/Python.h

このあたり。


