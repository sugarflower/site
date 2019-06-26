# Python全般

現在自分自身を呼び出している「親」を知る

ただしあくまでこれで分かるのは親であって親が呼び出している他のモジュールなどを参照することは出来ない。

```python
import inspect
parent = inspect.getmodule(inspect.stack()[1][0])
```

