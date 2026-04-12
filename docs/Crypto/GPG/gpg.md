GnuPG is a complete and free implementation of the OpenPGP standard as defined by RFC4880 (also known as PGP).

## 1. Usage

### 1.1. 签名校验

下载程序及其签名文件 [Debian 12.12](https://cdimage.debian.org/cdimage/archive/12.12.0/amd64/iso-dvd/) 

```
SHA256SUMS
SHA256SUMS.sign
debian-12.12.0-amd64-DVD-1.iso
```

获取指纹

```
┌──(nemo@debian)-[~]
└─$ gpg --verify ./SHA256SUMS.sign ./SHA256SUMS
```

```
DF9B9C49EAA9298432589D76DA87E80D6294BE9B
```

导入公钥

```
┌──(nemo@debian)-[~]
└─$ gpg --keyserver keyring.debian.org --recv-keys DF9B9C49EAA9298432589D76DA87E80D6294BE9B
```

签名校验

```
┌──(nemo@debian)-[~]
└─$ gpg --verify ./SHA256SUMS.sign ./SHA256SUMS
```

```
Primary key fingerprint: DF9B 9C49 EAA9 2984 3258  9D76 DA87 E80D 6294 BE9B
```

访问 [Verifying authenticity of Debian images](https://www.debian.org/CD/verify) 确认指纹与发布在官网的相同

```
pub   rsa4096/DA87E80D6294BE9B 2011-01-05 [SC]
      Key fingerprint = DF9B 9C49 EAA9 2984 3258  9D76 DA87 E80D 6294 BE9B
uid                  Debian CD signing key <debian-cd@lists.debian.org>
```

计算 Hash 值并与 `SHA256SUMS` 中的内容进行对比

```
┌──(nemo@debian)-[~]
└─$ sha256sum ./debian-12.12.0-amd64-DVD-1.iso
```

```
PS C:\Users\nemo> certutil -hashfile .\debian-12.12.0-amd64-DVD-1.iso SHA256
```

```
84aae16b26c8d2802d1e327eb84ab8f053f0aa68b0b1679f5e6aaffd4fe549cc
```

列出公钥

```
┌──(nemo@debian)-[~]
└─$ gpg --list-keys
```

删除公钥

```
┌──(nemo@debian)-[~]
└─$ gpg --delete-key DF9B9C49EAA9298432589D76DA87E80D6294BE9B
```

---

References

- [GnuPG](https://www.gnupg.org/)
- [MIT Key Server](https://pgp.mit.edu/)
- [OpenPGP Key Server](https://keys.openpgp.org/)
- [Ubuntu Key Server](https://keyserver.ubuntu.com/)
