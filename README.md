# Java Tips

### 1. Some quick Java tips:

* The && operator computes the boolean and of two expressions, while || computes boolean or.
* The % operator implements remainder. For example, the value of x % 4 will be 0, 1, 2, or 3. （负数取模，整数倍是0，其他仍然是负数：-15 % 10 = 5，-10 % 10 = 0）
* The == and != operators compare two values for inequality. The code fragment if (n % 4 == 1) reads as “if the remainder when dividing n by 4 is equal to 1.”
* In Java, there are 8 primitive types: byte, short, int, long, float, double, boolean, and char

### CS61B Commit

#### 1, 代理设置：

（1）查看代理：

```
git config --global --get http.proxy
git config --global --get https.proxy
```

（2）取消代理：

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

目前测试commit的结果是：取消代理，然后打开v2ray软件，就可以正常连接到git hub

#### 2，git 修改push到git hub

（1）官方的方法：

```
git status
git add lab01/magic_word.txt
git commit -m "Added Magic Word"
git push origin main
```

（2）实际的操作（第一次是这样，之后可以用官方的方法正常推送修改）：

```
git add lab01/magic_word.txt
git commit -a
(这里会跳出来编辑器，编辑commit的log，修改之后保存)
git push origin main
```

### 3. Debug

* F7: Step Into
* F8: Step Over
* F9: Continue
