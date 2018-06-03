[TOC]

# Python生成hash摘要（hashlib）

## hash是什么？

​	Hash，一般翻译做“散列”，也有直接音译为“哈希”的，就是把任意长度的输入(又叫做预映射pre-image)通过散列算法变换成固定长度的输出，该输出就是散列值。这种转换是一种压缩映射，也就是，散列值的空间通常远小于输入的空间，不同的输入可能会散列成相同的输出，所以不可能从散列值来确定唯一的输入值。**简单的说就是一种将任意长度的消息压缩到某一固定长度的消息摘要的函数**。 （准确的说hash并不是加密，加密就意味着解密，而hash算法生成的消息摘要是不可逆的，准确的应该说是数字签名或数字摘要）

## 常见的hash算法

### MD5

**MD5**即Message-Digest Algorithm 5（信息-摘要算法5），用于确保信息传输完整一致。是计算机广泛使用的杂凑算法之一（又译摘要算法、哈希算法），主流编程语言普遍已有MD5实现。将数据（如汉字）运算为另一固定长度值，是杂凑算法的基础原理，MD5的前身有MD2、MD3和MD4。 

### SHA家族

**安全散列算法**（英语：Secure Hash Algorithm，缩写为SHA）是一个密码散列函数家族，是FIPS所认证的安全散列算法。能计算出一个数字消息所对应到的，长度固定的字符串（又称消息摘要）的算法。且若输入的消息不同，它们对应到不同字符串的机率很高。 

SHA家族的五个算法，分别是SHA-1、SHA-224、SHA-256、SHA-384，和SHA-512，由美国国家安全局(NSA)所设计，并由美国国家标准与技术研究院（NIST）发布；是美国的政府标准。后四者有时并称为SHA-2。 

## 通过Python生成hash摘要

```python
from hashlib import md5, sha1, sha224, sha256, sha384, sha512

hash_md5 = md5()  # MD5 hash对象
hash_sha1 = sha1()  # SHA1 hash对象
hash_sha224 = sha224()  # SHA224 hash对象
hash_sha256 = sha256()  # SHA256 hash对象
hash_sha384 = sha384()  # SHA384 hash对象
hash_sha512 = sha512()  # SHA512 hash对象

def main():
    my_str = 'TengTengCai Is A Good Boy.'
    print('将要加密的字符串:', my_str)
    
    # MD5 加密
    h_md5 = hash_md5.copy()  # 复制一个对象，避免频繁创建对象消耗性能
    h_md5.update(my_str.encode('utf-8'))  # 需要将字符串进行编码，传递二进制数据
    md5_str = h_md5.hexdigest()  # 获取16进制的摘要
    print('MD5加密结果:',md5_str)  # 输出结果

    # SHA1 加密
    h_sha1 = hash_sha1.copy()
    h_sha1.update(my_str.encode('utf-8'))
    sha1_str = h_sha1.hexdigest()
    print('SHA1加密结果:', sha1_str)

    # SHA224 加密
    h_sha224 = hash_sha224.copy()
    h_sha224.update(my_str.encode('utf-8'))
    sha224_str = h_sha224.hexdigest()
    print('SHA224加密结果:', sha224_str)

    # SHA256 加密
    h_sha256 = hash_sha256.copy()
    h_sha256.update(my_str.encode('utf-8'))
    sha256_str = h_sha256.hexdigest()
    print('SHA256加密结果:', sha256_str)

    # SHA384 加密
    h_sha384 = hash_sha384.copy()
    h_sha384.update(my_str.encode('utf-8'))
    sha384_str = h_sha384.hexdigest()
    print('SHA384加密结果:', sha384_str)

    # SHA512 加密
    h_sha512 = hash_sha512.copy()
    h_sha512.update(my_str.encode('utf-8'))
    sha512_str = h_sha512.hexdigest()
    print('SHA512加密结果:', sha512_str)


if __name__ == '__main__':
    main()

```

```bash
[tianjun@localhost test]$ python3 example01.py 
将要加密的字符串: TengTengCai Is A Good Boy.
MD5加密结果: f22e9f250679c291143712c882a4d3fb
SHA1加密结果: d0e9dfb4dae1c0c441012c5fd6d0122ee6aa861f
SHA224加密结果: 3495968ffea6cccd590cf7101033adb59ee79630e4b0a73cea6b166b
SHA256加密结果: 5e16342562e9808342a18ffcf55f49c369984c1ac959b74a2883fcc16281aa7c
SHA384加密结果: d061d1dbd957048a6988accb41f6cf39032011d66f01af4ec8d2e7cda6c8a36f80e59bcbda726fde6ddb2e5882765ff7
SHA512加密结果: 76244c0c4a3938337acb9182bb9eda44e35e741e44e08afe4fe229b65350849f5a2beba189c5b58504c1a47f54cab8d484bf0f9eb3e981e8aabc5f0eec2b6b51
```





