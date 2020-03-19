# 符号

## 各种括号

**不同尺寸的括号**

使用\big, \Big, \bigg, \Bigg 或者 \left \right

```latex
\Bigg ( 
  \bigg [ 
     \Big \{ 
     		\big \langle 
     			\left | 
     				\| \frac{a}{b} \| 
     			\right | 
     		\big \rangle 
     \Big \} 
  \bigg ] 
\Bigg )
```

$$
\large{  \Bigg (     \bigg [        \Big \{           \big \langle             \left |               \| \frac{a}{b} \|             \right |           \big \rangle        \Big \}     \bigg ]   \Bigg )}
$$

**其它种类括号**

| 功能                         | 语法                                                       | 显示                                                         |
| :--------------------------- | :--------------------------------------------------------- | :----------------------------------------------------------- |
| 圆括号，小括号               | \left**(** \frac{a}{b} \right**)**                         | $\large{\left( \frac{a}{b} \right)}$                         |
| 方括号，中括号               | \left**[** \frac{a}{b} \right**]**                         | ![\left[ \frac{a}{b} \right]](http://upload.wikimedia.org/math/8/5/8/8585c96f355f7e301fd5143bea32efaf.png) |
| 花括号，大括号               | \left**\{** \frac{a}{b} \right**\}**                       | ![\left\{ \frac{a}{b} \right\}](http://upload.wikimedia.org/math/c/4/d/c4d4af6bab9a0e6532dddd50e7d27158.png) |
| 角括号                       | \left **\langle** \frac{a}{b} \right **\rangle**           | ![\left\langle \frac{a}{b} \right \rangle](http://upload.wikimedia.org/math/d/0/6/d06e733ce705ed26a7e048dbd2945371.png) |
| 单竖线，绝对值               | \left**\|** \frac{a}{b} \right**\|**                       | ![\left\| \frac{a}{b} \right\|](http://upload.wikimedia.org/math/4/0/d/40d6c8253b08e8801a01b3f6e5069a62.png) |
| 双竖线，范                   | \left **\|** \frac{a}{b} \right **\|**                     | ![\left \| \frac{a}{b} \right \|](http://upload.wikimedia.org/math/f/3/0/f30a5c412d1e4b4e7c6195ff5d47e947.png) |
| 取整函数 （Floor function）  | \left **\lfloor** \frac{a}{b} \right **\rfloor**           | ![\left \lfloor \frac{a}{b} \right \rfloor](http://upload.wikimedia.org/math/c/0/7/c07e1fc7c0150828e55da4efe37e8a3f.png) |
| 取顶函数 （Ceiling function) | \left **\lceil** \frac{c}{d} \right **\rceil**             | ![\left \lceil \frac{c}{d} \right \rceil](http://upload.wikimedia.org/math/8/6/8/868c1e52c339e01204aa1a77d44e3c71.png) |
| 斜线与反斜线                 | \left **/** \frac{a}{b} \right **\backslash**              | ![\left / \frac{a}{b} \right \backslash](http://upload.wikimedia.org/math/2/f/3/2f3c5907c0a4fc4fda69eb71890ce952.png) |
| 上下箭头                     | \left **\uparrow** \frac{a}{b} \right **\downarrow**       | ![\left \uparrow \frac{a}{b} \right \downarrow](http://upload.wikimedia.org/math/6/b/f/6bf93fc6691c8f753ced64787f531fe0.png) |
|                              | \left **\Uparrow** \frac{a}{b} \right **\Downarrow**       | ![\left \Uparrow \frac{a}{b} \right \Downarrow](assets/d246f8868b72ca62f5ab8e106be1d193.png) |
|                              | \left **\updownarrow** \frac{a}{b} \right **\Updownarrow** | ![\left \updownarrow \frac{a}{b} \right \Updownarrow](assets/4038e550c5ea5ac8a2cb62252156b8c2.png) |
| 混合括号                     | \left [ 0,1 \right )  <br/>\\left\langle \psi \right \|    | $\left [ 0,1 \right )$<br>$ \left \langle \psi \right\langle$ |
| 单左括号                     | \left \{ \frac{a}{b} **\right .**                          | ![\left \{ \frac{a}{b} \right .](http://upload.wikimedia.org/math/c/e/d/ced2a2fb558fe49fa56018b9f8fd69d5.png) |
| 单右括号                     | **\left .** \frac{a}{b} \right \}                          | ![\left . \frac{a}{b} \right \}](assets/9ac9b3c6d21c56f5b2b474b0ea1c4b8a.png) |





# 公式

## 多行对齐

```latex
\begin{equation}
  \begin{split}
    x&=a+b+c\\
    &=d+e\\
    &=f+g
  \end{split}
\end{equation}
```

$$
\large{
  \begin{equation}
    \begin{split}
      x&=a+b+c\\
      &=d+e\\
      &=f+g
    \end{split}
  \end{equation}
}
$$

## 大括号

**左侧大括号**

```latex
f(x)=
  \begin{cases}
    0 & if & \text{x=0}\\
    1 & if & \text{x!=0}
  \end{cases}
```

$$
\large{
  f(x)=
  \begin{cases}
    0 & if & \text{x=0}\\
    1 & if & \text{x!=0}
  \end{cases}
}
$$

