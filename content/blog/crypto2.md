---
title: "串流加密法(Stream ciphers)"
date: 2021-10-30T10:07:47+06:00
draft: false


# meta description
description: "串流加密法"

# taxonomies
categories:
  - "密碼學"
tags:
  - "密碼學"


# post type
type: "featured"
---
上次我們說完 OTP，這次我們來說說讓 OTP 更能更實際應用的方法，也就是串流加密法。

串流加密這個想法並不是完全隨機的密鑰，而是利用一個叫做 偽隨機密鑰(pseudo-random key)，要解釋他如何運作，要先定義一下 偽隨機生成器(pseudo-random generator)，簡稱 PRG。

PRG 是一個函數，這個函數 G 將 s 位數的 bit 映射到 n 位數的 bit ，其中 n 遠大於 s，可以想像 {0, 1}^s 這個種子空間為 128 bits 而 {0, 1}^n 可能是 GB 等級的。

```
 G: {0, 1}^s -> {0, 1}^n where n >> s
```

也就是說在這邊給一個 s 位的 input 值 k 經過 G 函數後產生出一個 n 位 output 值 G(k)，在這邊真正隨機的是 k，G(k) 值則是"看起來"隨機。

所以說加解密變成這樣
```
c = E(k, m) := m ⊕ G(k)
D(k, c) := c ⊕ G(k)
```

問題:
串流加密法是否為 perfect secrecy？
當然不是，因為他的 key 長度小於 message 長度。那麼他還是安全的嗎？這邊需要給出另一個"安全"的定義，來解釋為何串流加密法是安全的。

### PRG 必須是無法預測的

假設 PRG 是可預測的，則存在一個 i 使得 G(k)|1, ..., i
，--alg--> G(k)|i+1,...,n。
意思是當我們知道 G(k) 的前 i 項，就可以推出剩餘的項，也代表 PRG 是可預測的，進而代表他不安全。

為什麼呢，假設有一個攻擊者攔截到密文，且攻擊只知道這段訊息的某些內容，例如我們知道在郵件協議( stmp )，在網路上用標準郵件呼叫最一開始會用冒號，所以他知道訊息開頭片段了，所以將密文前半段與訊息前半段做 xor 可以得到 G(k) 的前半段，又因為他知道前半段就可以推斷出後半段，他就推出整個 G(k) 了，借而推斷出原訊息。

所以如果 G:K->{0, 1}^n 是可預測的，則
```
∃ "efficient alg." A and ∃ 1 ≤ i ≤ n-1 s.t. 
choose a ramdon k ∈ K , Pr[A(G(k))|1,...,i = G(k)|i+1] ≥ 1/2 + ε (ε ≥ 1/2^30)
```

note. 可不可忽略(Negligible and non-negligible)
- ε ≥ 1/2^30 不可忽略(可能發生在 1GB 大小之中)
- ε ≥ 1/2^80 可忽略

定義:
PRG is unpredictable if it is not predictable
```
=> ∀i: no "eff" adv. can predict bit (i + 1) for "non-neg" ε
```

### 不好的 PRGs
1. Linear Congruential generator

有三個參數a, b, p 其中 a,b 是整數，而 p 是質數，然後我們定 r[0] 為種子生產器

接著根據 r[i] = a * r[i - 1] + b mod p
算完產生後，i++，繼續往下算...

舉例：
m = 123, a = 5, b = 2 , r[0] = 73
r[1] = 5 * 73 + 2 = 367 (mod 123) = 121 (mod 123)
r[2] = 5 * 121 + 2 =  607 (mod 123) = 115 (mod 123)

...
所以生成一段隨機數字 73, 121, 115, ...

然而這是很容易去預測的，只要給多一些輸出值，就能推算出規律

2. glibc random():

r[i] <- (r[i - 3] + r[i - 31]) % 2^32

output r[i] >> 1

這些都不要用！

