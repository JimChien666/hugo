---
title: "對稱性加密(Symmetric Ciphers)"
date: 2021-10-30T10:07:47+06:00
draft: false


# meta description
description: "OTP 概念簡述"

# taxonomies
categories:
  - "密碼學"
tags:
  - "密碼學"


# post type
type: "featured"
---
定義: 
```
a cipher defined over (K, M, C) is a pair of "efficient" algs (E, D) 
where E: K * M -> C, D: K * C -> M ,
s.t. ∀ m ∈ M, k ∈ K: D(k, E(k, m)) = m
```
在 (K, M, C) 上定義的密碼是一對"高效率"演算法 (E, D)，其中 E: K * M -> C, D: K*C -> M ,使其對於 M 中的所有 m，K 中的所有 k：D(k, E(k, m)) = m

- E 通常是個隨機演算法
- D 通常是個確定性演算法

### 一次性加密(The One Time Pad)

明文 M 跟 密文 C 都是長度為 n 個 bit

M = C = {0, 1}^n

K = {0, 1}^n

Key 是一個隨機且長度同樣為 n 個 bit

C := E(k, m) = k ⊕ m

m: 0 1 1 0 1 1 1
k: 1 0 1 1 0 0 1
⊕
c: 1 1 0 1 1 1 0

D(k, E(k, m)) = D(k, k ⊕ m) = k ⊕ (k ⊕ m) = k ⊕ k ⊕ m = m

問題：給你一個 訊息 m 和 OTP 加密後的密文 c，可以算出 OTP 的密鑰 k嗎？

答案：可以，密鑰 k 為 m ⊕ c

### OTP 不實用

當訊息 m 長度太長的時候，密鑰 k 也會很長，所以當 Ann 要和 Bob 對談時，Ann發送消息給 Bob，他傳送一條訊息，就要生成一條跟訊息一樣長的密鑰 k，但雖然不實用， OTP 卻很安全，怎麼樣才能說得上安全呢，我們得先定義"安全性"。

### 密碼算法的安全性(Information Theoretic Security)
#### (shannon 1949)

基礎想法: 密文不應該透露任何有關原文的消息。

定義: 
A cipher (E, D) over (K, M, C) has **perfect secrecy** if

```
∀ m0, m1 ∈ M (len(m0) = len(m1)) and ∀ c ∈ C

Pr[E(k, m0) = c] = Pr[E(k, m1) = c]

where k si uniform in K (k <-r K)
```
這個意思是當我拿到一串密文 c 時，我不能確定他的原文是 m0 還是 m1，因為他們的機率相等，所以攻擊者無法用密文進行攻擊。

Lemma: OTP has **perfect secrecy**

證明:
1. 前情提要：若一組明密文 m, c 的密鑰總數為常數，則 這個加密為 perfect secrecy
```
∀ m, c: Pr[E(k, m) = c] = (#keys k ∈ K s.t. E(k, m) = c)/|K|

so: ∀ m, c: #{k ∈ K: E(k, m) = c} = const.

=> cipher has perfect secrecy
```
2. 證明 OTP 為 perfect secrecy
```
For OTP: if E(k, m) = c
=> k ⊕ m = c => k = m ⊕ c

=>#{k ∈ K: E(k, m) = c} = 1  ∀ m, c

=> OTP has perfect secrecy

```

結論:
之前學到的替換加密法，因為英文字母的出現頻率，很容易透露出密文與原文之間的關係，但在 OTP 中，你無法找到密文與原文之間的關係。因為當你獲得密文 c 時，他的原文 m0, m1 機率相等，所以具有安全性。但是，還是有其他方法可能攻破這個 OTP，再他的密鑰太長，不方便使用。那麼是否有比較短的密鑰 k 也具備安全性呢。這邊有個結論。

```
Thm: perfect secrecy => |K| >= |M|
```

這邊的意思是若要有 perfect secrecy，密鑰的長度必須大於原文的長度，也就是說，密鑰長度至少要等於原文長度，才有 perfect secrecy 特性。