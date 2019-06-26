# pyg / res

リソースをバイナリに固めて扱う。

現在のソース

```python
#!/usr/bin/python3
#import inspect

class Resource:

	def __init__(self):
		#self.parent = inspect.getmodule(inspect.stack()[1][0])
		pass

	def w2b(self, value):
		return bytes([ value & 0xff , (value & 0xff00) >> 8 ])

	def b2w(self, data):
		return int((data[1] << 8) | data[0])

	def d2b(self, data):
		return str(data).encode()

	def b2d(self, data):
		return eval(data.decode())

	def size(self, data):
		return w2b(len(data))

	def getSize(self, file):
		return b2w(file.read(2))

	def getHeader(self, file):
		size = getSize(file)
		head = file.read(size)
		return b2d(head)

	def test(self):
		data = {"res1":(0,100), "res2":(100,104)}
		d = d2b(data)
		with open("data.res","wb") as f:
			f.write(size(d))
			f.write(d)

	def readTest(self):
		with open("data.res","rb") as f:
			h = getHeader(f)
			print(h)
```

現状ヘッダを読むくらいまでしか出来ていない。

ヘッダの情報はリソース名（ID）、先頭アドレス、サイズ（バイト）となっている。

ヘッダの先頭２バイトはヘッダのサイズ（バイト）を書き込む。ヘッダデータが64Kbyteまでという制限がある。これは実際のリソースファイル全体の容量ではなくあくまでヘッダが64Kbyteまでということで。作っている本人も混乱したので忘れないように書いておく。

## ボディー読み込み部分を作らないといけない。

ということでその辺りもぼちぼちと
ていうかなんでinspectなんて入ってるんだ なにか実験してたのはうっすら記憶にあるけど…。
