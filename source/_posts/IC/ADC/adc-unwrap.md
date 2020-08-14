---
title: ADC 测试数据处理 ADC Test Data Processing
toc: true
date: 2020-08-13 11:15:08
tags:
	- ADC
	- Signal & System
---

## 插值采样 Decimation

### 采用插值采样的原因

考虑高速 ADC 测试直接将高频数据输出，而在片上设计存储器的方式需要较大的面积与精细的时序设计，因此通常采用 插值 (Decimation) 采样 的方法，用分频后的时钟采样锁存 ADC 的原始数据，得到降频后的数据，再通过 模拟 pad 输出片外测试。

### 插值采样的配置

若插值系数为 P_deci，则插值采样的时钟分频比应设为 P_deci。考虑相干采样和静态性能测试的要求，一般所需的有效数据点应不少于 2（N_bit+2） ，设为 N_fft，即片外仪器采集到 N_fft 个数据点，而对应时间内，ADC 输出的总数据点数应为 N_data= N_fft * P_deci，对应满足相干采样条件的输入信号频率为 F_in=max(primes(N_data/2))​。 

### 频谱混叠

由于内插采样，数据采样的频率实际是 F_s/P_deci，N_fft 个数据点对应的频率分辨率为 F_s/N_data，即频率分辨率不变，而对应的频率区间变为 F_s/P_deci，即频率区间缩小为 1/P_deci。实际的采样频率降低了，当 F_in > F_s/2/P_deci 时，会发生混叠，即输入信号位于第一奈奎斯特区间以外时，会混叠回第一奈奎斯特区间。其中，偶数区间的信号还会发生镜像。如，若无额外处理，F_in = 0.9 * F_s/2 的信号会显示在 0.1 * F_s 的位置。为了得到真实的频谱，需要对采用插值采样的数据进行处理：

1. 数据对应的频率区间范围缩小至 F_s/2/P_deci ；
2. 偶数奈奎斯特区间的信号在频域上镜像

## 数据展开 Unwrapping

相干采样的正弦波可以使用称为“展开”的概念重新组合。 

数据展开的基本原理是，采样所得的 第 i 个数据，是 N_fft 个点的单个周期波形数据中的第 mod( i*M, N_fft) 个，其中 N_fft 是所处理的数据点数，M 是所处理的数据中包含的输入信号周期数。

图8 显示了一个正弦波，其中 M = 11个 周期的输入信号采样 N = 4096次。 图9是应用展开后的相同波形。 如果已对波形进行了相干采样，则展开后的波形应看起来像是一个周期采样了N次。

![Fig8](https://i.loli.net/2020/07/19/TZqwke8uGrsDv4W.png)

![Fig9](https://i.loli.net/2020/07/19/MlouV6tS2eaqZsF.png)

## 仿真验证

用 matlab 脚本对理想 ADC 的数据进行 插值采样 和 展开。

对一个 8 bits 的 ADC，采样频率 F_s=1 GHz，处理点数 N_fft = 1024，插值系数为 32，ADC 产生的总数据点数 N_data=32768，包括 cycles=16381 个周期的输入信号，即 F_in=F_s/N_data * cycles=499.1 MHz，位于第 32 奈奎斯特区间，相关结果如下。

![Time Domain](https://i.loli.net/2020/08/14/wzdSRgjul41698f.png)



![image-20200814013000773](https://i.loli.net/2020/08/14/dDSgf6Otv8wFsZa.png)



![Unwrapping Output](https://i.loli.net/2020/08/14/ApoER86xCGHrNzO.png)

## References

> - [A Tutorial in Coherent and Windowed Sampling with A/D Converters [Renesas]](https://www.renesas.com/us/en/www/doc/application-note/an9675.pdf)



## 附录 相关算法

### 1. 算法伪代码

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

### 2. Matlab 代码

#### 2.1 De-Decimation (release aliasing)

```matlab
% fft spectrum
pfft = abs(fft(dout_deci))/N_fft/(VFS/2)^2*2; % normalize to full range
[p_row,p_col,p_v] = find(pfft);
p_base = min(p_v);
s = 10*log10(pfft + p_base*0.5);
s1 = s(1:N_fft/2);
s2 = s(N_fft/2+1:N_fft);

% decide Nyquist region
N_nq = ceil(Fin/(Fs/2/P_deci));
F_res = Fs / N_fft / P_deci;
flag_odd = mod(N_nq, 2);
if flag_odd 
    s_eq = s1;
else
    s_eq = s2;
end

% frequency domain
f = (0:N_fft/2-1) * F_res + (N_nq-1)* Fs/2/P_deci;
figure(2);
plot(f,s_eq, 'r');
hold on;
plot(f(i_sig), s_eq(i_sig),'r-*','LineWidth',3);
grid on
box on;
txt = [ 'F\_in = ', num2str(f(i_sig)/1e6,3),'MHz'];
text(f(i_sig-floor(0.1*N_fft)), s_eq(i_sig)-2,txt);
xlabel('Frequency[f/f_s]');
ylabel('Magnitude [dBFS]');
```



2.2 Unwrapping

````matlab
% unwrapping
% N_data: number of data needed to be captured
% N_fft: number of the data de-decimation, target data point
% N_data = N_fft * P_deci
dout_uw = zeros(N_fft,1);
Fin_uw = f(i_sig);
step_uw = round(Fin_uw / Fs * N_data);
for i_data=(1:N_fft)
    j_data = mod(((i_data-1)*step_uw + 1), N_fft); 
    if j_data == 0 j_data = N_fft; end
    dout_uw(j_data) = dout_deci(i_data);
end
````

