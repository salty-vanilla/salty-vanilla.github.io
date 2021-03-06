---
layout: post
title: Generative Probabilistic Novelty Detection with Adversarial Autoencoders
thumbnail: 2018-10-10-Generative Probabilistic Novelty Detection with Adversarial Autoencoders/architecture.png
published: false
---

## TL;DR
* 今までのAutoEncoderを使ったanomaly detection のアプローチは入力と再構成画像の再構成誤差だけで行っていた
* それに加えて，未知データがどのくらい正常データセットに属するかという確率的なアプローチを盛り込んだ
* Autoencoder の構造に潜在変数用のDiscriminatorと再構成画像ようのDiscriminatorを付加した

## Generative Probabilistic Novelty Detection
訓練用データポイント集合 $x_1, ..., x_N$ $(x \in \mathbb{R}^m)$ というのは，以下のモデルからサンプリングされている．ただし，$\xi_i$はノイズ成分

$$
\begin{equation}
x_i = f(z_i) + \xi_i
\end{equation}
$$

ここで，$z \in \Omega \subset \mathbb{R}^n$であり，変換関数$f$は$f:\Omega \to \mathbb{R}^n$であり，多様体$M$は$M \equiv f(\Omega)$である．ただし，$n < m$．  

また，$g: \mathbb{R}^m \to \mathbb{R}^n$かつ$f(g(x))=x$を満たす逆変換関数$g$がすべての$x \in M$について存在することを仮定する．

テスト時には，$\bar{x} \in \mathbb{R}^m$がモデル(1)からサンプリングされたかを調べたい．なので，まず以下の計算を行う．

$$
\bar{z} = g(\bar{x}) \\
\bar{x^\parallel} = f(\bar{z})
$$

ここで，$f(z)$をTaylor展開

$$
f(z) = f(\bar{z}) + J_f(\bar{z})(z - \bar{z}) + O(|z - \bar{z}|^2)
$$

$J_f(\bar{z})$はヤコビ行列．  
$\bar{x^\parallel}$における$f$の接ベクトル空間Tは$T=span(J_f(\bar{z}))$ と表せる．  
$T$のイメージは以下の図

<img src="">

$J_f(\bar{z})$ を特異値分解して，$J_f(\bar{z}) = U^{\parallel}SV^T$とすると，$T=span(U^{\parallel})$となる．
$U^{parallel}$はrank: nであり，$U = [U^{\parallel}U^{\perp}]$がユニタリ行列であるような$U^{\perp}$を定義すると，  
