# Python全般

## 現在自分自身を呼び出している「親」を知る


```python
import inspect
parent = inspect.getmodule(inspect.stack()[6][0])
```

注意点として、親がまだ参照出来ていないモジュールやファンクションにはアクセス出来ないというタイミング的な話が出てくる。

おおよそメインのスクリプトから「親」を参照するように指示があってから参照、実行すれば問題は無いと思う。

<br>

<br>

## Python.hの在り処

バージョン
Linux raspberrypi 4.19.42-v7+ #1219 SMP Tue May 14 21:20:58 BST 2019 armv7l GNU/Linux

- /usr/include/python2.7/Python.h
- /usr/include/python3.5m/Python.h

このあたり。

```g+ -I/usr/include/python3.5m soruce.cpp -o out```

という感じで使えばいい。

