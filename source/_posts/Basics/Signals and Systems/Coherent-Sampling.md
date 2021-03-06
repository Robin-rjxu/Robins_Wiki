---
title: 相干采样 Coherent Sampling
toc: true
date: 2020-07-18 13:11:54
tags:
	- DSP
	- Signals & Systems
---

## 关键

- 采样窗口中存在整数个周期性波形，称为相干采样。
- 通过相干采样可以确保，单频率输入信号的频谱中，信号功率包含在一个 FFT 谱线内，避免频谱泄露。
- 使用素数 M 个输入信号周期，可以消除了采样点数 N 的所有公因数，避免不同次谐波混叠后位于相同谱线，保证谐波性能计算的准确性。
- 输入信号，采样时钟频率的准确性对测试结果有显著影响，体现为 M 的变化导致 ENOB 测试结果的显著恶化。
- 相干采样并不保证采集数据的完备性，即 N 个采样点不一定覆盖所有输出数字码，而是与输入信号的概率分布特性有关。




## 概述

### 背景 

相干采样 是评估高速数据转换器动态性能的最有效工具之一。满足一定条件下，这一既有可以提高 FFT 的频谱分辨率，消除使用 窗口采样 的需要。然而，如果相干采样的条件不满足，可以使用 窗口采样。

FFT 把采集的时域数据转换到频域时，假设从 -∞ 到 +∞ 连续采样，即假设采样到的信号为周期信号。如果不满足上述用于相干采样的条件，则可能造成信号起点/终点的非连续性，发生非相干采样。除非使用窗口采样，将采样波形与描述窗口的在时域相乘，来补偿波形的不连续性，否则频谱泄漏是不可避免的。

### 相干采样的定义

当采样窗口中存在整数个周期时，就会实现周期性波形的相干采样，即满足下式：

$$
\dfrac{f_{IN}}{f_S} = \dfrac{M}{N}
$$

其中：

> $f_S$  为采样频率
>
> $f_{IN}$ 为输入频率
>
> M  为数据周期数
>
> N  为采样的点数

数据周期数 M 通常选择2以外的质数，这确保了采样信号数据不会在 N 个采样点数内周期重复。

通过相干采样可以确保，单频率输入信号的频谱中，信号功率包含在一个 FFT 谱线内。

## 讨论

### 数据周期数的选择

数据周期数 M 通常选择2以外的质数，这确保了采样信号数据不会在 N 个采样点数内周期重复。

例如，如果 N = 32 且 M = 6（非质数），则输入频率的每个周期将有 N / M = 5.33 个采样，因此输入的三个周期将恰好获取16个采样，因此前16个采样样本将与后16个样本相同。通常，通过重复相同的采样点不会获得任何其他信息，因此通常不应使用非质数 M 。

### 相干采样的理想参数

对于任意M和N，相干关系都成立，但实际值可提供更好的结果。由于其固有的周期性，FFT要求采样数为2的幂，因此 N 通常选择 2 的幂。 DFT可以在任意样本大小上执行，但需要更多的计算时间。 M应该是奇数或质数。使用 奇数的 M，可以消除了 N 的许多公因数；若使用素数 M，可以消除了所有 N 的公因数。M和N之间的公因数可能导致混叠后， FFT 的频谱上，$f_{IN}$ 的不同次谐波位于相同的谱线而  M 的唯一性对于谐波失真的计算绝对必要。

下面讨论什么条件下，不同次谐波位于相同谱线。

若 谐波 $h$ 在 N 点 FFT 的频谱中的谱线位于 $M_h$ ，即满足：

$$
M_h = \begin{cases} |h-N\times \lfloor \dfrac{h + N/2}{N} \rfloor |, \ h\neq \dfrac{N}{2} \\ 0, \ h=\dfrac{N}{2} \end{cases}
$$

假设不同次谐波 $h_1$ 和 $h_2$ 位于同一谱线，则满足 $M_{h1} = M_{h2} = M_{h}$ ，且 $h_1, h_2, N$ 均为正整数，则有

$$
\dfrac{h_1 \pm h_2}{N} = \lfloor \dfrac{h_1 + N/2}{N} \rfloor \pm \lfloor \dfrac{h_2 + N/2}{N} \rfloor
$$

即

$$
M_{h1} = M_{h2} \Leftrightarrow \dfrac{h_1 \pm h_2}{N} = integer
$$

由于第 $x$ 次谐波满足 $h = M \cdot x$， 即周期 $M$ 的整数倍，上式还可以进一步简化为 

$$
(x_1 \pm x_2)\cdot M/N = integer
$$

例如，若 N = 64，M = 24，则 $h_{10} = 16$，$h_{14} = 16$，此时计算谐波的功率将存在误差。

上式表明，若谐波位置与 FFT 点数之比为整数，不同谐波会出现在统一谱线上。若采样数据的周期数 M 与 N 互质（无公因数），则上式必不成立。

考虑经典的 $f_{IN}=f_S/2$ 的情况，由于 $M=N/2$，式(6) 条件变为 $(x_1 \pm x_2)/2 = integer$。这意味着，所有的奇次谐波都与 基频 位于相同谱线，而所有偶数谐波都位于 DC 处，因此所得频谱中不包括任何谐波和信噪比信息。

### 相干采样的非理想参数

上述讨论说明，考虑测试设备的频率分辨率和线性度性能，实现相干采样的条件比较复杂，通常需要对采样时钟和输入信号的频率进行繁琐的迭代计算才可求得。

小幅调整 M 的 0.005 以改变相干条件，则谐波分量会显着增加，并且会发生称为展开的情况。 展开（有时称为拖尾或泄漏）会导致以基频频率点为中心的尖峰。 尖峰的宽度表示不连贯的程度。 对于理解问题的本质至关重要的概念是，将 M 改变的真正含义理解为 $f_{IN}$ 相对 $f_S$ 相对变化。 如果 N 和 $f_S$ 保持恒定，并且 M 增加 0.005，则输入频率 $f_{IN}$ 增加 $f_S / N * 0.005$，从而在时域窗口中引起泄漏，导致非相干性。

A/D 测试的一个重要指标是 ENOB 或有效位数。图6 显示了 M 从 M-0.5 变为 M + 0.5 对 ENOB 性能的影响。由于数据基于理想转换器，因此我们希望能够实现10位精度。确实，存在一个可以达到10位的区域，但是它很小。该区域的敏锐度突出了为什么必须准确观察到连贯性的原因。假设输入频率为 10MHz，图6 的范围代表了 9995117Hz 至 10004883Hz 的输入频率范围。当 M 的变化进一步缩小为 ±0.00005，图7 显示了 M 的变化为 0.0005 时 ENOB 的变化。在大多数情况下，高频设备的可编程精度为 1Hz，从而实现 ±0.5Hz 所需的 (M±0.0005)的范围。信号源的精度使理想输入信号时达到在 9.95-10.0 位范围。

![fig6](https://i.loli.net/2020/07/19/qOelhfDsGJtomSC.png)

![image-20200719004921060](https://i.loli.net/2020/07/19/AwOgVEkKte1BJC9.png)

### 波形展开

相干采样的正弦波可以使用称为“展开”的概念重新组合。 图8 显示了一个正弦波，其中 M = 11个 周期的输入信号采样 N = 4096次。 图9是应用展开后的相同波形。 如果已对波形进行了相干采样，则展开后的波形应看起来像是一个周期采样了N次。

![Fig8](https://i.loli.net/2020/07/19/TZqwke8uGrsDv4W.png)

![Fig9](https://i.loli.net/2020/07/19/MlouV6tS2eaqZsF.png)

### 重采样与插值重采样

如果采集了9.5个周期的波形，则可以缩短数据窗口以忽略额外的0.5个周期。假设原始数据集的长度为1045个样本，如图10所示，在丢弃了额外的0.5个周期后，数据集将减少为990个样本，如图11所示。现在可以对修改后的数据执行990点DFT，以产生无泄漏的准确结果。这给限于2的幂 FFT算法带来问题。

![Fig10-11](https://i.loli.net/2020/07/19/2XliOcWvKthx7kT.png)

但是，可以对新样本集进行重新采样以适合给定的样本大小，或者可以应用插值技术将波形拟合到适当大小的样本集中。图11 中的 9.0 个周期波形被重新采样，提供1024个采样数据点。

图15显示了线性重采样在频域中的影响。线性重采样可保留原始数据的幅度并避免泄漏，但会产生更不连续的波形，从而导致谐波失真。

图14是一个类似的示例，只是数据使用线性插值进行了重采样。线性插值重采样可以解决时域不连续的问题，但不能完全保留原始数据。

![Fig14-15](https://i.loli.net/2020/07/19/iOMC7gn1W5XpP3h.png)

### 仿真实例

#### 输入为正弦信号

由于正弦信号并非均匀分布，对 B 位理想 ADC，满足相干采样，采集 $N=2^B$ 个点，并不能获得输入范围内的所有数字码。

例如，取一个 8 位理想 ADC

- 采样 256 个点
  - M = 127 时，得到 134 个数字码， ENOB = 7.38；
  - M = 11时，得到 200 个数字码， ENOB = 8.2。
- 采样 512 个点
  - M = 251 时，得到 214 个数字码， ENOB = 7.95；
  - M = 11 时，得到 256 个数字码， ENOB = 7.93。
- 采样 1024 个点
  - M = 127 时，得到 256 个数字码， ENOB = 7.99；
  - M = 11时，得到 256 个数字码， ENOB = 7.97。

#### 输入为斜波信号

由于斜波信号为均匀分布，对 B 位理想 ADC，满足相干采样，采集 $N=2^B$ 个点，足以获得输入范围内的所有数字码。

例如，取一个 8 位理想 ADC

- 采样 256 个点
  - M = 127 时，得到 256 个数字码；
  - M = 11时，得到 256 个数字码。
- 采样 512 个点
  - M = 251 时，得到 256 个数字码；
  - M = 11 时，得到 256 个数字码。
- 采样 1024 个点
  - M = 127 时，得到 256 个数字码；
  - M = 11时，得到 256 个数字码。

#### 小结

即使采用了相干采样的激励条件，考虑实际输入信号的概率分布特性，并非所有满摆幅的信号形式都可以在 N 个采样点内获得全部数字码的结果。这一特性对 ADC 动态性能的评估误差影响可能较小，因为其频谱结果给出的频域特性信息完备。但对于 ADC 的静态性能评估影响较大，影响失码的评估等基于数据概率密度的测试结果。

因此，通常建议对 ADC 的静态性能测试，采集多于 $4\cdot 2^B$ 的数据点，以获得更准确的性能；同时考虑，采样点数增加的过采样响应对 ADC 信噪比性能的影响，正确评估测试结果与 ADC 的实际性能的关系。

## References
> - [Coherent Sampling vs. Window Sampling [Maxiam]](https://www.maximintegrated.com/en/design/technical-documents/tutorials/1/1040.html)
> - [A Tutorial in Coherent and Windowed Sampling with A/D Converters [Renesas]](https://www.renesas.com/us/en/www/doc/application-note/an9675.pdf)
> - [Coherent sampling [Wikipedia]](https://en.wikipedia.org/wiki/Coherent_sampling)

## 附录 相关算法

```C
void coherence(f_in,f_s,N,M)
double *f_in,*f_s;
int N,*M;
{
int K;
K = (*f_s + N/2)/N;
*M = (int)(*f_in)/(int)K/2*2+1;
*f_s = K*N;
*f_in = K*(*M);
}
/* Sample Call */
/* coherence(&f_in,f_s,N,M); */
}/* End of Coherence Algorithm */
```

```C
void unwrap_algorithm(tsample,unwrap,size_cap,M)
int tsample[],unwrap[],M,size_cap;
{
int i,j;
for (i=0; i<size_cap;i++)
{
j = M*i % size_cap;
unwrap[j] = tsample[i];
}
/* Sample Call */
/* unwrap_algorithm(tsample,unwrap,size_cap,f_bin); */
}/* End of Unwrap Algorithm */
```

```C
void alias_algorithm(fbin,N)
int *fbin,N;
{
*fbin=fabs((float) ( *fbin - N *((*fbin+N/2)/N) ) );
if (*fbin == N/2) *fbin = 0;
/* Sample Call */
/* alias_algorithm(&fbin,M); */
}/* End of Alias Algorithm */
```