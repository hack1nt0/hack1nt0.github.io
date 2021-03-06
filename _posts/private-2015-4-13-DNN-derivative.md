---
layout: post
title: DNN求导
---

# {{page.title}}

==============

{{page.date | date_to_string}} - Beijing

**注：下文的专有名词如果不见特别限定(标量、矩阵)，都表示向量；符号右上标表示其属于哪一层，右下标表示其哪一维度，左上标表示其属于哪一个训练样本。** **DNN的结构：**[Norm, Sigmod, Softmax, CrossEntropy(CE)]**

符号系统：$$z^i$$ -> 第i层的输入，$$a^i$$ -> 第i层的输出，$$W^i$$ -> 第i层的变换矩阵，$$b^i$$ -> 第i层的偏置，$$L(lables)$$ -> 先验类别布尔向量

![Image.pic](http://ofb2ysj1w.bkt.clouddn.com/long%E8%BD%AC%E5%9E%8Bdouble%E5%A4%B1%E8%B4%A5.tiff?imageView/1/w/800/h/600)

## Forward Propagation:

$$ z^0 = x^0 $$

$$a^0 = norm(z^0)$$

$$z^1 = a^0 * W^1 + b^1$$

$$a^1 = sigmod(z^1)$$

$$z^2 = a^1 * W^2 + b^2$$

$$a^2 = softmax(z^2)$$

$$z^3 = a^2$$

$$a^3 = CE(z^3, L)$$

## Cost Function:

$$f(x, W, b, L) = a^3 $$

## Back Propagation:

Layer 3:

$$\frac{\partial f}{\partial z^3}$$

$$= \frac{\partial CE(z^3, L)}{\partial z^3}$$

$$= \frac{\partial{\sum_i{z^3_i * log(L_i)}}}{\partial z^3}$$

$$= [-\frac{L_i}{a^3_i}] {1 * colN(z^3)}$$

$$= \delta_3$$

Layer 2:

$$\frac{\partial f}{\partial z^2}$$

$$= \frac{\partial f}{\partial z^3} * \frac{\partial z^3}{\partial z^2}$$

$$= \delta_3 * \frac{\partial z^3}{\partial z^2} $$

$$= \delta_3 * \frac{\partial softmax(z^2)}{\partial z^2}$$

$$= \delta_3 * Diag_{rowN(z^2) _colN(z^2)} {e[i, i] = softmax(z^2, i)(1 - softmax(z^2, i))} $$

$$= \delta_2$$

--------------------------------------------------------------------------------

$$\frac{\partial f}{\partial W^2}$$

$$=\frac{\partial f}{\partial z^2} * \frac{\partial z^2}{\partial W^2}$$

$$=\delta_2 \frac{\partial (a^1 W^2 + b2)}{\partial{W^2}}$$

$$=\delta_2 * [a^1]^T_{colN(W^2)*1}$$

--------------------------------------------------------------------------------

$$\frac{\partial{f}}{\partial{b^2}}$$

$$=\frac{\partial{f}}{\partial{z^2}} * \frac{\partial{z^2}}{\partial{b^2}}$$

$$=\delta_2 \frac{\partial{a^1 W^2 + b2}}{\partial{b^2}}$$

$$=\delta_2$$

Layer 1:

$$\frac{\partial{f}}{\partial{z^1}}$$

$$=\frac{\partial{f}}{\partial{z^2}} * \frac{\partial{z^2}}{\partial{z^1}}$$

$$=\delta_2 * \frac{\partial{z^2}}{\partial{z^1}} $$

$$=\delta_2 * \frac{\partial{sigmod(z^1)}}{\partial{z^1}}$$

$$=\delta_2 \times sigmod(z^1) \times (1 - sigmod(z^1))$$

$$=\delta_1$$

--------------------------------------------------------------------------------

$$\frac{\partial{f}}{\partial{W^1}}$$

$$=\frac{\partial{f}}{\partial{z^1}} * \frac{\partial{z^1}}{\partial{W^1}}$$

$$=\delta_1 \times \frac{\partial (a^0 W^1 + b1)}{\partial{W^1}}$$

$$=\delta_1 * [a^0]^T_{colN(W^1)*1}$$

--------------------------------------------------------------------------------

$$\frac{\partial{f}}{\partial{b^1}}$$

$$=\frac{\partial{f}}{\partial{z^1}} * \frac{\partial{z^1}}{\partial{b^1}}$$

$$=\delta_1 \times \frac{\partial (a^0 W^1 + b1)}{\partial{b^1}}$$

$$=1 \delta_1 $$
