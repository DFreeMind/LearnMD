# 字体大小设置
```
{\tiny           Hello}
{\scriptsize     Hello}
{\small          Hello}
{\normalsize     Hello}
{\large          Hello}
{\Large          Hello}
{\LARGE          Hello}
{\huge           Hello}
{\Huge           Hello}
```
$$
{\tiny           Hello}\\
{\scriptsize     Hello}\\
{\small          Hello}\\
{\normalsize     Hello}\\
{\large          Hello}\\
{\Large          Hello}\\
{\LARGE          Hello}\\
{\huge           Hello}\\
{\Huge           Hello}\\
$$

$$
\small{hello}\\
\tiny{hello}\\
\normalsize{hello}
$$

# 公式编号
使用 `\tag{...}` 生成块级公式编号。

$$
x^n+y^n=z^n \tag{1.1}
$$

多个公式成一个号：
$$
\begin{split}
x^2+y^2=z^2 \\
x^3+y^3=z^3 \\
x^n+y^n=z^n 
\end{split}
\tag{1.2}
$$

# 换行与对齐
换行使用 `\\`，公式对齐使用 `&` 并配合 `\begin{split} ... \end{split}` 使用

$$
\begin{split}
a &= b \\
c &= d \\
e &= f 
\end{split} \tag{1.3}
$$

有时候需要罗列多个公式，可以用`eqnarray*`标签包围公式代码，在需要转行的地方使用`\\`，每行需要使用2个`&`来标识对齐位置，两个`&...&`号之间的是公式间对齐的位置，每行公式后可使用`\tag{...}`标签编号：
```
\begin{eqnarray}
x^n+y^n &=& z^n \tag{1.4} \\
x+y &=& z \tag{1.5}
\end{eqnarray}
```

$$
\begin{eqnarray}
x^n+y^n &=& z^n \tag{1.4} \\
x+y &=& z \tag{1.5}
\end{eqnarray}
$$

# 添加空格

```
 两个quad空格	a \qquad b	$a \qquad b$	两个m的宽度
 quad空格	a \quad b	$a \quad b$	一个m的宽度
 大空格	a\ b	$a\ b$	1/3m宽度
 中等空格	a\;b	$a\;b$	2/7m宽度
 小空格	a\,b	$a\,b$	1/6m宽度
 没有空格	ab	$ab\,$	
 紧贴	a\!b	$a\!b$	缩进1/6m宽度
 ```

# 括号的使用
```
y=
\begin{cases}
x, & x>0,\\
-x, & x \le 0
\end{cases}
```
$$
y=
\begin{cases}
x, & x>0,\\
-x, & x \le 0
\end{cases}
$$

# 左右排列公式
```
\left.
\begin{aligned}
a &= b+c \\
d &= b+c
\end{aligned}
\right.
\Longrightarrow
\qquad a=d
```
$$
\left.
\begin{aligned}
a &= b+c \\
d &= b+c
\end{aligned}
\right.
\Longrightarrow
\qquad a=d
$$

或者
$$
\begin{equation}
\left.
\begin{aligned}
a &= b+c \\
d &= b+c
\end{aligned}
\right\}
\Longrightarrow
\qquad a=d
\end{equation}
$$

# 曾广矩阵
```
\large{
\left[
    \begin{array}{c:c}
        \begin{matrix}
            a_{11}&\cdots&a_{1n}\\
            \vdots&\ddots&\vdots\\
            a_{m1}&\cdots&a_{mn}
        \end{matrix}&
        
        \begin{matrix}
            b_1\\
            \vdots\\
            b_m
        \end{matrix}
    \end{array}
\right]
}
```
$$
\large{
\left[
    \begin{array}{c:c}
        \begin{matrix}
        a_{11}&\cdots&a_{1n}\\
        \vdots&\ddots&\vdots\\
        a_{m1}&\cdots&a_{mn}
        \end{matrix}&
        \begin{matrix}
        b_1\\
        \vdots\\
        b_m
        \end{matrix}
    \end{array}
\right]
}
$$

# 字体
原始字体为：$x,A,B$
- 加粗加斜`\boldsymbol{}`，效果为 $\boldsymbol{x,A,B}$
- 只加粗`\mathbf{}`，效果为$\mathbf{x,A,B}$
- 可筛选加粗方式 `\pmb{}`，效果为$\pmb{x,A,B}$
- 可筛选加粗方式 `\mathrm{}`，效果为$\mathrm{x,A,B}$

# 分段函数
```
sign(x)=\begin{cases}
1,&x>0 \\ 0,&x=0 \cr -1,&x<0
\end{cases}
```
$$
sign(x)=\begin{cases}
1,&x>0 \\ 0,&x=0 \cr -1,&x<0
\end{cases}
$$

# 标注

## 等号上标注

`\overset{def}{=}`: $\overset{def}{=}$
