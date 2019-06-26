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


## こうなった。

```
#!/usr/bin/python3

class Resource:

	def __init__(self,res=""):
		if res !="":
			self._resFile = res
			with open(self._resFile,"rb") as f:
				data = f.read(2)
				self._size = int((data[1] << 8) | data[0])
				head = f.read(self._size)
				self._index = eval(head.decode())

	def get(self,resName):
		_idx,_size = self._index[resName]
		_idx += self._size + 2
		with open(self._resFile,"rb") as f:
			f.seek(_idx)
			data = f.read(_size)
		return data
```

一回しか呼ばれてないとかいうファンクションに意味はないなと思い。

まあまあいい感じかなぁと思う。

次はバイナリに固める方を作ってゆこう。

## 固める方

```python
#!/usr/bin/python3
import argparse, os

p = argparse.ArgumentParser(description="usage: ./pack_resource.py inDir [-o outFile]")
p.add_argument("inDir", help="asset directory" )
p.add_argument("--outFile","-o", default="out.dat", help="output binary file name")
args = p.parse_args()
assetDir = args.inDir
outFile  = args.outFile
if outFile == "":
	outFile = "out.dat"

d = os.listdir(assetDir)
h = {}
binCount = 0
for i in d:
	if os.path.isfile(assetDir + os.sep + i):
		size = os.stat(assetDir + os.sep + i)[6]
		h[i] = (binCount,size)
		binCount += size

headBin = str(h).encode()
value = len(headBin)
headSize = bytes([ value & 0xff , (value & 0xff00) >> 8 ])

with open(outFile,"wb") as f:
	f.write(headSize)
	f.write(headBin)
	for i in d:
		if os.path.isfile(assetDir + os.sep + i):
			with open(assetDir + os.sep + i,"rb") as f2:
				data = f2.read()
				f.write(data)

```

ということで泥臭く実装。

assetディレクトリなんていうのを作って固めたいファイルをいろいろ投げ込んで ```./pack_resource.py assetディレクトリ名```　みたいな感じで使う。```-o``` オプションで書き出すファイル名も指定できる。

これで関連ファイルがゴソゴソあるっていう気持ち悪い状況からは脱せそうです。