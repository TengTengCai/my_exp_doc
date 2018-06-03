# Python对数据进行简单压缩处理

在Python丰富的库中，也有着对数据进行压缩处理的库（zlib）。对于需要数据压缩的应用程序，此模块中的功能允许使用zlib库进行压缩和解压缩。 （本文只对简单的字符串数据进行压缩，如需压缩文件等复杂数据类型，详见[zlib](http://www.zlib.net/)官网进行更详细的学习）

Python3的字符串类型为Unicode,而非字节。

对Unicode字符串进行压缩，先得将字符串编码为字节形式，通过`zlib.compress()`方法压缩字节数据。

对压缩后的字节数据进行解压缩，通过`zlib.decompress()`方法解压缩字节数据，再将字节数据解码为Unicode字符串

在这里我们将Python之禅进行压缩和解压缩处理

```python
import zlib
import this


def main():
    python_zen = this.s  # 获取Python之禅的Unicode字符串
    com_bytes = zlib.compress(python_zen.encode('utf-8'))  # 编码为UTF-8格式的字节进行压缩
    print(com_bytes)
    decom_bytes = zlib.decompress(com_bytes)  # 将压缩的字节进行解压缩
    print(decom_bytes.decode('utf-8'))  # 将解压缩的字节进行UTF-8解码得到Unicode字符串


if __name__ == '__main__':
    main()

```

在这里我们好像看不出什么效果来，我们将数据存储在文件中，查看文件大小来区分压缩和未压缩。

```python
import this
import zlib


def main():
    python_zen = this.s  # 获取字符
    with open('data.txt', 'wb') as f:  # 使用文件写入的上下文环境
        f.write(python_zen.encode('utf-8'))  # 写入未压缩的字节数据

    with open('com_data.txt', 'wb') as f:  # 使用文件写入上下文环境
        com_zen = zlib.compress(python_zen.encode('utf-8'))  # 将字符串编码并压缩
        f.write(com_zen)  # 写入压缩后的字节数据


if __name__ == '__main__':
    main()

```

运行代码看看压缩结果吧

```bash
[tianjun@192 zlib_example]$ python3 example06.py 
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
[tianjun@192 zlib_example]$ ll
总用量 12
-rw-rw-r--. 1 tianjun tianjun 445 6月   4 00:16 com_data.txt
-rw-rw-r--. 1 tianjun tianjun 856 6月   4 00:16 data.txt
-rw-rw-r--. 1 tianjun tianjun 505 6月   4 00:15 example06.py
```

可以清楚的看到原数据`data.txt`大小为856字节，压缩后`com_data.txt`大小为445字节，由此可见压缩效果非常的好，压缩了接近50%。



