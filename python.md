# Python全般

## 現在自分自身を呼び出している「親」を知る


```python
import inspect
parent = inspect.getmodule(inspect.stack()[n][0])
```

呼び出し方によりnが変動する場合があるので注意が必要。
親から直接呼ばれた自身から親を参照する場合、多分 n = 2 だと思う。

"""
c = 0
for s in inspect.stack():
	print( c, s , "\n" )
	c += 1
"""

などと実際に動かしてみて```filename=```親モジュール と有る行が何番目か確認するのが確実だけれども、そういったところでかなり扱いが難しいので多用は無用。いらないトラブルのもとになる。

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

