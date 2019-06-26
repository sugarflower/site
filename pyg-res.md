# pyg / res

リソースをバイナリに固めて扱う。

現在のソース

```python
#!/usr/bin/python3

class Resource:

	def __init__(self,res=""):
		if res !="":
			self._resFile = res
			with open(self._resFile,"rb") as f:
				self._index = self.getHeader(f)

	def get(self,resName):
		_idx,_size = self._index[resName]
		_idx += self._size + 2
		with open(self._resFile,"rb") as f:
			f.seek(_idx)
			data = f.read(_size)
		return data

	def w2b(self, value):
		return bytes([ value & 0xff , (value & 0xff00) >> 8 ])

	def b2w(self, data):
		return int((data[1] << 8) | data[0])

	def d2b(self, data):
		return str(data).encode()

	def b2d(self, data):
		return eval(data.decode())

	def size(self, data):
		return self.w2b(len(data))

	def getHeader(self, file):
		self._size = self.b2w(file.read(2))
		head = file.read(self._size)
		return self.b2d(head)

	def test(self):
		data = {"res1":(0,100), "res2":(100,104)}
		d = self.d2b(data)
		with open("data.res","wb") as f:
			f.write(self.size(d))
			f.write(d)
			f.write(bytes([0xff]*100))
			f.write(bytes([0x55]*104))

```

半端な状態で放置されていたので一気に動くように修正した。

データを作る処理については *test* の通り。いちいちプログラム書くのは面倒なのでassetディレクトリの中身をリソースファイルにまとめるようなツールを作っておいたほうがよさそう。
