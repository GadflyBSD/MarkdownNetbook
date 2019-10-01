[TOC]

> ## 简要介绍
>
> 因为要使用markdown编辑数学公式然后在文档、网页中使用，但是自己总是记不住这么多东西，特别是几天不用，很多都忘记了，因此准备就数学公式的Latex编辑方式做一个整理，以方便自己和读者今后使用。

### 常用数学运算符 
| 名称     | 算式                                                         | markdown                                                     |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 加       | $x+y$                                                        | `	$ x + y $`                                              |
| 减       | $	x−y $                                                   | `	$ x - y $`                                              |
| 乘       | $ x \times y $                                               | `$ x \times y`                                               |
| 点乘     | $ x \cdot y $                                                | `$ x \cdot y $`                                              |
| 星乘     | $ x \ast y $                                                 | `$ x \ast y $`                                               |
| 除       | $ x \div y $                                                 | `$ x \div y $`                                               |
| 分数     | $ \frac{x}{y} $                                              | `$ \frac{x}{y} $`                                            |
| 上标     | $ x ^ y $                                                    | `$ x ^ y $`                                                  |
| 下标     | $ x _ y $                                                    | `$ x _ y $`                                                  |
| 开二次方 | $ \sqrt x $                                                  | `$ \sqrt x $`                                                |
| 开方     | $ \sqrt[x]{y^4+3y-1} $                                       | `$ \sqrt[x]{y^4+3y-1} $`                                     |
| 加减     | $ x \pm y $                                                  | `$ x \pm y $`                                                |
| 减加     | $ x \mp y $                                                  | `$ x \mp y $`                                                |
| 行间公式 | $ \frac{d}{dx}e^{ax}=ae^{ax}\quad \sum_{i=1}^{n}{(X_i - \overline{X})^2} $ | `$ \frac{d}{dx}e^{ax}=ae^{ax}\quad \sum_{i=1}^{n}{(X_i - \overline{X})^2} $` |
| 开根号   | $\sqrt{2};\sqrt[n]{3}$                                       | `$\sqrt{2};\sqrt[n]{3}$`                                     |
| 矢量     | $\vec{a} \cdot \vec{b}=0$                                    | `$\vec{a} \cdot \vec{b}=0$`                                  |
| 积分     | $\int ^2_3 x^2 {\rm d}x$                                     | `$\int ^2_3 x^2 {\rm d}x$`                                   |
| 极限     | $\lim_{n\rightarrow+\infty} n$                               | `$\lim_{n\rightarrow+\infty} n$`                             |
| 累加     | $∑1i2	\sum \frac{1}{i^2}$                                 | `$∑1i2	\sum \frac{1}{i^2}$`                               |
| 累乘     | $\prod \frac{1}{i^2}$                                        | `$\prod \frac{1}{i^2}$`                                      |

* ### 关系运算符
| 名称       | 算式             | markdown           |
| ---------- | ---------------- | ------------------ |
| 等于       | $ x = y $        | `$ x = y $`        |
| 小于等于   | $ x \leq y $     | `$ x \leq y $`     |
| 大于等于   | $ x \geq y $     | `$ x \geq y $`     |
| 不大于等于 | $ x \ngeq y $    | `$ x \ngeq y $`    |
| 不大于等于 | $ x \not\geq y $ | `$ x \not\geq y $` |
| 不等于     | $ x \neq y $     | `$ x \neq y $`     |
| 约等于     | $ x \approx y $  | `$ x \approx y $`  |
| 恒等于     | $ x \equiv y $   | `$ x \equiv y $`   |
| 省略号     | $ \cdots $       | `$ \cdots $`       |

* ### 逻辑运算符
| 名称 | 算式          | markdown     |
| ---- | ------------- | ------------ |
| 因为 | $\because$    | \because     |
| 所以 | $\therefore$  | \therefore   |
| ∀    | $\forall$     | \forall      |
| ∃    | $\exists$     | \exists      |
| ≠    | $\not=$       | \not=        |
| ≯    | $\not>$       | \not>        |
| ⊄    | $\not\subset$ | $\not\subset |

* ### 集合运算符
| 算式         | markdown  | 算式        | markdown  | 算式        | markdown  |
| ------------ | --------- | ----------- | --------- | ----------- | --------- |
| $\emptyset $ | \emptyset | $\in $      | \in       | $\notin $   | \notin    |
| $\subset $   | \subset   | $\supset $  | \supset   | $\subseteq$ | \subseteq |
| $\supseteq$  | \supseteq | $\bigcap$   | \bigcap   | $\bigcup$   | \bigcup   |
| $\bigvee$    | \bigvee   | $\bigwedge$ | \bigwedge | $\biguplus$ | \biguplus |
| $\bigsqcup$  | \bigsqcup |             |           |             |           |

* ### 希腊字母
| 大写     | markdown | 小写       | markdown | 小写          | markdown    | 小写       | markdown |
| -------- | -------- | ---------- | -------- | ------------- | ----------- | ---------- | -------- |
| $\alpha$ | \alpha   | $\beta$    | \beta    | $\Gamma$      | \Gamma      | $\gamma$   | \gamma   |
| $\Delta$ | \Delta   | $\epsilon$ | \epsilon | $\varepsilon$ | \varepsilon | $\zeta$    | \zeta    |
| $\eta$   | \eta     | $\Theta$   | \Theta   | $\theta$      | $\theta     | $\iota$    | \iota    |
| $\kappa$ | \kappa   | $\Lambda$  | \Lambda  | $\lambda$     | \lambda     | $\mu$      | \mu      |
| $\nu$    | \nu      | $\Xi$      | \Xi      | $\xi$         | \xi         | $\omicron$ | \omicron |
| $\Pi$    | \Pi      | $\pi$      | \pi      | $\rho$        | \rho        | $\Sigma$   | \Sigma   |
| $\sigma$ | \sigma   | $\tau$     | \tau     | $\Upsilon$    | \Upsilon    | $\upsilon$ | \upsilon |
| $\Phi$   | \Phi     | $\phi$     | \phi     | $\varphi$     | \varphi     | $\chi$     | \chi     |
| $\Psi$   | \Psi     | $\psi$     | \psi     | $\Omega$      | \Omega      | $\omega$   | \omega   |

* ### 戴帽符号：
| 算式      | markdown | 算式        | markdown  | 算式        | markdown  |
| --------- | -------- | ----------- | --------- | ----------- | --------- |
| $\hat{y}$ | \hat{y}  | $\check{y}$ | \check{y} | $\breve{y}$ | \breve{y} |

* ###连线符号： 
| 算式                  | markdown           | 算式                   | markdown                                       |
| --------------------- | ------------------ | ---------------------- | ---------------------------------------------- |
| $\overline{a+b+c+d} $ | \overline{a+b+c+d} | $\underline{a+b+c+d} $ | $\overbrace{a+\underbrace{b+c}_{1.0}+d}^{2.0}$ |

* ### 箭头符号：
| 算式             | markdown       | 算式              | markdown        | 算式              | markdown        |
| ---------------- | -------------- | ----------------- | --------------- | ----------------- | --------------- |
| $\uparrow$       | \uparrow       | $\downarrow$      | \downarrow      | $\Uparrow$        | \Uparrow        |
| $\Downarrow$     | \Downarrow     | $\rightarrow$     | \rightarrow     | $\leftarrow$      | \leftarrow      |
| $\Rightarrow$    | \Rightarrow    | $\Leftarrow$      | \Leftarrow      | $\longrightarrow$ | \longrightarrow |
| $\longleftarrow$ | \longleftarrow | $\Longrightarrow$ | \Longrightarrow | $\Longleftarrow$  | \Longleftarrow  |

* ### 使用指定字体
| 名称       | 算式                   | markdown             |
| ---------- | ---------------------- | -------------------- |
| 罗马体     | ${\rm longleftarrow}$  | {\rm longleftarrow}  |
| 意大利体   | ${\it longleftarrow}$  | {\it longleftarrow}  |
| 黑体       | ${\bf longleftarrow}$  | {\bf longleftarrow}  |
| 花体       | ${\cal longleftarrow}$ | {\cal longleftarrow} |
| 倾斜体     | ${sl longleftarrow}$   | {\sl longleftarrow}  |
| 等线体     | ${\sf longleftarrow}$  | {\sf longleftarrow}  |
| 数学斜体   | ${\mit longleftarrow}$ | {\mit longleftarrow} |
| 打字机字体 | ${\tt longleftarrow}$  | {\tt longleftarrow}  |


* ### 三角函数
| 名称     | 算式                  | markdown             |
| -------- | --------------------- | -------------------- |
| 垂直     | $\bot $               | `\bot `              |
| 角       | $\angle $             | \angle               |
| 度数     | $30^\circ$            | 30^\circ             |
| 正弦函数 | $\sin A=\frac{x}{y} $ | `\sin A=\frac{x}{y}` |
| 余弦函数 | $\cos A=\frac{x}{y} $ | `\cos A=\frac{x}{y}` |
| 正切函数 | $\tan A=\frac{x}{y} $ | `\tan A=\frac{x}{y}` |
| 余切函数 | $\cot A=\frac{x}{y} $ | `\cot A=\frac{x}{y}` |
| 正割函数 | $\sec A=\frac{x}{y} $ | `\sec A=\frac{x}{y}` |
| 余割函数 | $\csc A=\frac{x}{y} $ | `\csc A=\frac{x}{y}` |

* ### 对数函数
| 名称    | 算式      | markdown  |
| ------- | --------- | --------- |
| ln函数  | $\ln15$   | `\ln15`   |
| log函数 | $log_210$ | `log_210` |
| lg函数  | $lg7$     | `lg7`     |

* ### 微积分运算符： 
| 算式        | markdown  | 算式       | markdown | 算式     | markdown |
| ----------- | --------- | ---------- | -------- | -------- | -------- |
| $y\prime x$ | y\prime x | $\int $    | \int     | $\iint $ | \iint    |
| $\iiint $   | \iiint    | $\iiiint $ | \iiiint  | $\oint $ | \oint    |
| $\lim $     | \lim      | $\infty $  | \infty   | $\nabla$ | \nabla   |

* ### 需要转义的字符
要输出字符　空格　#　$　%　&　_　{　}　，用命令：　\空格　#　\$　\%　\&　_　{　}