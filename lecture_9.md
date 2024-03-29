# Лекция 9. Методы проектирования дискретного БИХ-фильтра на основе аналогового фильтра прототипа

---


Пусть имеется некоторый аналоговый фильтр с импульсной характеристикой $h_a(t)$ и соответствующей ей передаточной функцией $H_a(p)$. При этом частотная характеристика этого фильтра определяется как функция частоты $\tilde H_a(f)=H_a(i\omega)=H_a(i2\pi f)$.

Таким образом, если на вход этого фильтра подается аналоговый сигнал $x(t)$, частотный спектр которого есть $X_a(i\omega)$, то выходной сигнал $y(t)$  и его частотный спектр $Y_a(i\omega)$ будут равны
$$
y(t)=\int_0^t x(t')h_a(t-t')dt'
$$
$$
Y_a(i\omega)=H_a(i\omega)X_a(i\omega)
$$
соответственно.

---

Пусть теперь произведена дискретизация сигнала $x(t)$ путем выборки его значений с шагом $\tau$:
$$
x[k]=x(k\tau)
$$
Задача состоит в том, чтобы спроектировать дискретный фильтр с импульсной характеристикой $h_d[k]$ и соответствующей ей передаточной функцией $H_d(z)$, при прохождении сигнала $x[k]$ через который  получался бы дискретный сигнал, мало отличающийся от 
$$
y[k]=y(k\tau)
$$
т.е. от дискретного сигнала, получающегося в результате дискретизации выхода аналогового фильтра непосредственно.

Т.е. требуется найти последовательность $h_d[0], h_d[1],...,h_d[n],...$ (вообще говоря, бесконечную), свертка с которой 
$$
\sum_{m=0}^{+\infty}x[m]h_d[k-m] \approx y[k]=y(k\tau)
$$
---

При этом в явном виде последовательность $\{h_d[k]\}$ может может потребоваться определять только если имеет конечную длину. В этом случае говорят о проектировании КИХ-фильтра. В случае фильтров с бесконечной импульсной характеристикой, т.е. БИХ-фильтров, вместо этого требуется определить коэффициенты соответствующей передаточной функции, которое представляется в виде дробно-рациональной функции 
$$
H_d(z)=\frac{b_0+b_1z^{-1}+...+b_Mz^{-M}}{1+a_1z^-1+...+a_Nz^-N}
$$
При этом передаточная функция $H_d(z)=\sum_{k=0}^{+\infty}h[k]z^{-k}$ - есть z-преобразование дискретной импульсной характеристики $\{h_d[k]\}$.

Коэффициенты этой передаточной функции есть коэффициенты в соответствующей линейной вычислительной схеме. Например, так называемая каноническая структурная схема дискретного фильтра имеет вид.
![[Pasted image 20240209083609.png]]
Существуют и другие эквивалентные структурные схемы БИХ-фильтров, но все они определяются коэффициентами передаточной функции.

---

Обычно проектирование аналогового цифрового БИХ-фильтра осуществляется на основе аналогового фильтра-прототипа.

Эта задача может решаться разными методами (и с разными результатами). Мы рассмотрим следующие два основных метода.

- Метод дискретизации импульсной характеристики аналогового фильтра (метод инвариантной импульсной характеристики).
- Метод билинейного преобразования передаточной функции аналогового фильтра в передаточную функцию дискретного фильтра.



Методы проектирования КИХ фильтров мы пока не рассматриваем, они проектируются иначе.




---
## Метод инвариантной импульсной характеристики

Пусть передаточная функция аналогового фильтра-прототипа представлена дробно-рациональной функцией. И пусть эта передаточная функция имеет $N$ полюсов $p_1,...,p_N$, причем, для простоты будем считать, что кратных полюсов нет. Тогда эта передаточная функция может быть представлена в виде суммы простых дробей
$$
H_a(p)=\sum_{k=1}^N\frac{r_k}{p-p_k}
$$
где $r_k$ - это вычет функции $H_a(p)$ в точке $p=p_k$ (при каждом $k=1,...,N$).

---

Отсюда, переходя от изображения по Лапласу к оригиналу, найдем выражение соответствующей импульсной характеристики аналогового фильтра

$$
h_a(t)=\sum_{k=1}^N
r_k e^{p_k t}I_{\mathbb R^+}(t)
$$
где $I_{\mathbb R^+}(t)$ - это функция Хевисайда.

Здесь мы воспользовались свойством линейности преобразования Лапласа и тем, что
$$
e^{at}I_{\mathbb R^+}(t) \to \frac{1}{p-a}
$$

---

Произведем теперь дискретизацию найденной импульсной характеристики по формуле
$$
h[m]=\tau h(m\tau)
$$
т.е. получим выражение искомой дискретной импульсной характеристики
$$
h[m]=\tau\sum_{k=1}^N r_k e^{p_km\tau}=\tau\sum_{k=1}^N r_k \big(e^{p_k\tau}\big)^m = \tau\sum_{k=1}^N r_k s_k^m
$$
при $s_k=e^{p_k \tau}$.

Чтобы найти передаточную функции получившегося дискретного фильтра, применим к его импульсной характеристике $z$-преобразование. Воспользовавшись при этом свойством линейности $z$-преобразования, а также тем известным фактом, что
$$
a^m \to \frac{1}{1-az^{-1}}\ \ \ (m=0,1,2,...)
$$
в результате получим
$$
H(z)=\tau \sum_{k=1}^N \frac{r_k}{1-s_kz^{-1}}
$$
Приведя эту сумму к общему знаменателю, найдем интересующие нас коэффициенты дроби
$$
H(z)=\frac{b_0+b_1z^{-1}+...+b_Mz^{-M}}{1+a_1z^-1+...+a_nz^-N}
$$
---

Важно отметить, что при использованном отображении $s_k=e^{p_k\tau}$ полюса устойчивого аналогового фильтра-прототипа, лежащие в левой p-полуплоскости, отображаются внутрь единичного круга с центром в начале координат z-плоскости. А это означает, что получаемый дискретный фильтр также является **устойчивым**.

Смысл названия метода **инвариантной импульсной характеристики** заключается в том, что импульсная характеристика соответствующего дискретного фильтра получается просто путем дискретизации импульсной характеристики аналогового фильтра-прототипа.

Рассмотрим теперь, к каким **погрешностям** будет приводить такой способ проектирования дискретного фильтра. Для этого вспомним связь частотной характеристики дискретного фильтра (полученного путем выборочной дискретизации) с частотной характеристикой аналогового фильтра-прототипа:
$$
H(i\omega)=\sum_{k \in \mathbb Z}H_a(i(\omega-2\pi k/\tau))
$$
Откуда видно, на частотах вблизи $\omega=2\pi/\tau$ будет иметь место так называемый эффект наложения.

![[Pasted image 20240208230013.png]]

Чем меньше будет выбрана величина $\tau$, тем в меньшей степени будет проявляться эффект наложения. Однако, для снижения эффекта наложения до приемлемого уровня могут понадобится слишком маленькие значения $tau$. 

Более детально исследовать рассматриваемое здесь отображение $z=e^{ptau}$ можно с помощью следующего программного кода на языке Julia.

```julia
using Plots

function map_plot(transformation::Function; σ_limits=(-2, 1), ω_max=5)
    # координатные линии на p-плоскости:
    ss_left = σ_limits[begin]:0.1:0 # - соответсвует левой полуплоскости
    ss_right = 0:0.1:σ_limits[end]  # - соответсвует правой полуплоскости
    ww = -ω_max:0.1:ω_max           # - диапазон рассматриваемых частот
                                    # p = s + i*w, s in ss, w in ww

    plt = plot(legend = false, ratio = :equal) # масштабы осей графика установлены равными 

    # семейство образов вертикальных линий  левой половины p-плоскости
    for s in ss_left # radius_cyrcle = exp(s)
        xx = [real(transformation(complex(s,w))) for w in ww]
        yy = [imag(transformation(complex(s,w))) for w in ww]
        plot!(plt, xx,yy)
    end
    # семейство образов вертикальных линий  правой половины p-плоскости
    for s in ss_right # radius_cyrcle = exp(s)
        xx = [real(transformation(complex(s,w))) for w in ww]
        yy = [imag(transformation(complex(s,w))) for w in ww]
        plot!(plt, xx,yy, linestyle = :dot)
    end

   # семейство образов горизонтальных линий  p-плоскости
    for w in ww
        xx = [real(transformation(complex(s,w))) for s in ss_left]
        yy = [imag(transformation(complex(s,w))) for s in ss_left]
        plot!(plt,xx,yy) # - график горизонтальной линии левой половины p-плоскости

        xx = [real(transformation(complex(s,w))) for s in ss_right]
        yy = [imag(transformation(complex(s,w))) for s in ss_right]
        plot!(plt,xx,yy, linestyle = :dot) # - график горизонтальной линии правой половины p-плоскости
    end

    s = 0 # - соответсвует мнимой оси p-плоскости
    xx = [real(transformation(complex(s,w))) for w in ww]
    yy = [imag(transformation(complex(s,w))) for w in ww]
    plot!(p, xx,yy, linewidth = 2, linecolor = :red)  # - график образа мнимой оси (оси частот) 
    
    scatter!(plt, (1, 0), markercolor = :red, markerstrokecolor = :red, markersize = 3)
    return plt 
end       
```

С помощью этого кода может быть построен следующий график, содержащий образы координатных линий p-плоскости при рассматриваемом отображении $z=e^{p\tau}$. 

```julia
τ = 1 # - период дискретизации сигнала

map_plot(p->exp(p*τ)) # - случай z = exp(p*τ)
savefig("map_exp.png")
```

![[map_exp.png]]

Сдвоенные радиальные линии фактически показывают, что здесь имеет место многозначное отображение p-плоскости на z-плоскость (имеет место наслоение образов p-плоскости).

## Метод билинейного преобразования передаточной функции аналогового фильтра-прототипа (в передаточную функцию дискретного фильтра)

Рассмотрим теперь другой метод проектирования дискретного фильтра по аналоговому фильтру-прототипу, свободному от недостатка метода инвариантной импульсной характеристики, обусловленного проявлением эффекта наложения.
$$
z=e^{p\tau}
$$
Этот метод, позволяет получить передаточную функцию дискретного фильтра непосредственно из передаточной функции аналогового фильтра-прототипа путем отображения p-плоскости в z-плоскость по формуле
$$
z=\frac{1+p\tau/2}{1-p\tau/2}
$$
Это отображение принято называть билинейным отображением p-плоскости в z-плоскость (не путать с билинейной функцией, рассматриваемой в линейной алгебре). 

Обратное отображение z-плоскости в р-плоскость тоже имеет вид билинейного отображения
$$
p=\frac{2}{\tau}\frac{z-1}{z+1}=\frac{2}{\tau}\frac{1-z^{-1}}{1+z^{-1}}
$$
Тогда, чтобы получить искомое выражение передаточной функции дискретного фильтра из выражения передаточной функции фильтра-прототипа, надо просто переменную $p$ выразить через $z^{-1}$, воспользовавшись обратным билинейным отображением: 
$$
H(z)=H_a\Big(\frac{2}{\tau}\frac{1-z^{-1}}{1+z^{-1}}\Big)
$$
При этом, если считать коэффициенты передаточной функции аналогового фильтра-прототипа известными, то, тем самым, и коэффициенты соответствующего дискретного фильтра можно считать определенными. При этом ясно, что порядок дискретного фильтра получился равным порядку фильтра-прототипа (так же как и в случае применения метода инвариантной импульсной характеристики).

Остается только понять, к каким именно искажениям выходного дискретного сигнала будет приводить так спроектированный дискретный фильтр. Для этого необходимо рассмотреть свойства билинейного преобразования.

Во-первых, заметим, что билинейное преобразование отображает левую p-полуплоскость внутрь единичного круга с центром в начале координат z-плоскости. А это означает, что устойчивому аналоговому фильтру-прототипу будет соответствовать так спроектированный дискретный фильтр.

Во-вторых, рассмотрим отображение оси частот, лежащей в p-плоскости, на соответствующую ей окружность единичного радиуса z-плоскости. В отличие от предыдущего случая, это отображение будет взаимно-однозначным. Это исключает возможность эффекта наложения, однако приводит к некоторому нелинейному искажению оси частот. 

Действительно, в выражении
$$
p=\frac{2}{\tau}\frac{z-1}{z+1}
$$
положим $p=i\omega$ и $z=e^{i\tilde\omega\tau}$, получим

$$
i\omega=\frac{2}{\tau}\frac{e^{i\tilde\omega\tau}-1}{e^{i\tilde\omega\tau}+1}=\frac{2}{\tau}\frac{e^{i\tilde\omega\tau/2}-e^{-i\tilde\omega\tau/2}}{e^{i\tilde\omega\tau/2}+e^{-i\tilde\omega\tau/2}}=i\frac{2}{\tau}\frac{\sin(\tilde\omega\tau/2)}{\cos(\tilde\omega\tau/2)}=i\frac{2}{\tau}\text{tg}(\tilde\omega\tau/2)
$$
т.е.
$$
\omega=\frac{2}{\tau}\text{tg}(\tilde\omega\tau/2)
$$
или
$$
\tilde\omega\tau=2\text{arctg}(\omega\tau/2)
$$
или, если для нормированных круговых "частот", ввести специальные обозначения: $\tilde w=\tilde \omega \tau$ и $w=\omega\tau$, то
$$
\tilde w = 2 \text{arctg}(w/2)
$$

Полученная формула выражает связь "аналоговой частотной шкалы" $w$ с "дискретной частотной шкалой" $\tilde w$. Эта связь между этими шкалами частот в данном случае оказалась нелинейной. Однако в окрестности нулевых частот эту связь между шкалами можно считать приблизительно линейной. 

![[Pasted image 20240208231341.png]]


Кроме того, что особенно важно, связь между рассматриваемыми шкалами частот является взаимно однозначной. Поэтому эффект наложения в этом случае отсутствует. Однако тут появляется эффект нелинейного отображения одной шкалы частот на другую, что в случае слишком высоких граничных частот фильтра-прототипа может приводить к существенным отклонениям частотной характеристики дискретного фильтра от предъявляемых к ней требований.

![[Pasted image 20240208232406.png]]

С помощью прежнего кода теперь может быть построен график, содержащий образы координатных линий p-плоскости при рассматриваемом билинейном отображении $z=(1+p\tau /2)/(1-p\tau /2)$. 

```julia
τ = 1 # - период дискретизации сигнала
# - случай z = (1+pτ/2)/(1-pτ/2)

map_plot(p->(1+p*τ/2)/(1-p*τ/2),  σ_limits = (-40, 1), ω_max = 100) 

savefig("map_bilinear.png")
```
![[map_bilinear.png]]

Билинейное преобразование дает однозначное отображение в z-плоскость. При этом, чем меньше значение периода дискретизации $\tau$, тем в более широком диапазоне частот между аналоговой и дискретной шкалами частот будет иметь место линейное соответствие с достаточно высокой точностью.

**Замечание 1.** Факт того, что рассматриваемые шкалы частот отображаются билинейной функцией взаимно однозначно, не отменяет того, что частотная характеристика дискретного фильтра является периодической функцией, период которой равен частоте дискретизации. Это только означает, что частотная характеристика фильтра-прототипа полностью сжимается (в результате нелинейной деформации оси частот) на один период (что, собственно, и исключает эффект наложения). 

**Замечание 2.** Факт отсутствия эффекта наложения в частотной характеристике дискретного фильтра, спроектированного методом билинейного преобразования, вовсе не означает, что эффект наложения будет отсутствовать в самом фильтруемом сигнале. Это только означает, что такой фильтр не внесет дополнительных искажений в сигнал, обусловленных эффектом наложения. Но если частота дискретизации была выбрана не достаточно большой, как того требует теорема Котельникова, то, разумеется, в частотном спектре входного дискретного сигнала уже будут присутствовать искажения, вызванные эффектом наложения. И от этих искажений последующая фильтрация избавить уже не сможет.

## Передаточные функции аналоговых БИХ-фильтров НЧ 

Для рассматриваемых здесь НЧ-фильтров частота среза по уровню $\sqrt 2/2$ принята равной 1 (можно считать также, что рассматривается нормированная на частоту среза "безразмерная частота"). 

### НЧ-фильтры Баттерворта 
$$
|H_n(i\omega)|^2=\frac{1}{1+\omega^{2n}}
$$

$$
H_n(p)=\prod_{k=1}^N\frac{1}{p-p_k}
$$
где полюса $p_k=...$ лежат в правой полуплоскости эквидистантно на единичной окружности с центром в начале координат.

![[Pasted image 20240208232836.png]]

### НЧ-фильтры Чебышева I рода
$$
|H_{n,\varepsilon}^I(i\omega)|^2=\frac{1}{1+\varepsilon^2T^2(\omega)}
$$
$$
H_{n,\varepsilon}(p)=\prod_{k=1}^N\frac{1}{p-p_k}
$$
где полюса $p_k=...$ лежат в левой полуплоскости на некотором эллипсе с центром в начале координат.
![[Pasted image 20240208233204.png]]
### НЧ-фильтры Чебышева II рода

$$
|H_{n,\varepsilon}^{II}(i\omega)|^2=\frac{1}{1+\varepsilon^2/T^2(1/\omega)}
$$

$$
H_{n,\varepsilon}^{II}(p)=1-H_{n,\varepsilon}^I(1-p)
$$

![[Pasted image 20240208233249.png]]

### Эллиптические НЧ-фильтры (фильтры Золотарева-Кауэра)
$$
|H_{n,\varepsilon,L}(i\omega)|^2=\frac{1}{1+\varepsilon^2 R^2(\omega,L\Big)}
$$

$R(\omega,L)$ - так называемая рациональная функция Чебышева.

![[Pasted image 20240208233440.png]]
### Фильтры Бесселя
$$
H_n(p)=\frac{d_0}{\sum_{k=0}^n d_k p^k}
$$
где
$$
d_k=2^{k-n}\frac{(2n-k)!}{k!(n-k)!}
$$

В знаменателе здесь находится так называемый многочлен Бесселя $n$-го порядка.

Преимуществом этого фильтра является практическое постоянство группового времени запаздывания в пределах его полосы пропускания по уровню $\sqrt 2/2$.

## Преобразования фильтров
### Преобразование, масштабирующее ось частот

$p \to p/\omega_0$

При этом, если речь идет о НЧ-фильтре или ВЧ-фильтре, то \omega_0$ - это требуемая граничная частота (частота среза).

Если же речь идет о полосовом или режекторном фильтре, то 
$$
\omega_0=\sqrt{\omega_Н \omega_В}
$$
где $\omega_В,\omega_Н$ - это верхняя и, соответственно, нижняя, частоты среза по уровню $\sqrt2/2$. При этом фактическая полоса пропускания полосового фильтра (полоса задержания режекторного фильтра) будет равна $\omega_В-\omega_Н=\omega_0/\Omega$, где $\Omega$ - соответствующий параметр нормированного полосового (режекторного) фильтра (см. ниже).


### Преобразования передаточных функций НЧ-фильтров в передаточные функции полосовых фильтров
$$
p \to (p+1/p)/\Omega
$$
где $\Omega>0$ - безразмерный параметр, равный ширине полосы пропускания по уровню $\sqrt 2/2$ нормированного по частоте полосового фильтра.

Это преобразование справедливо при условии, что $$
\omega_0=\sqrt{\omega_Н \omega_В}=1
$$В общем случае, при масштабировании переменной p, параметр $\Omega$ тоже должен быть масштабирован. 

Из формулы преобразования НЧ-фильтра в соответствующий полосовой фильтр порядок последнего удваивается.

![[Pasted image 20240208234037.png]]


### Преобразования НЧ-ВЧ, ПФ-РФ 
Преобразование НЧ-фильтра в ВЧ-фильтр (и наоборот), а также взаимное преобразование полосового фильтра в режекторный осуществляется с помощью следующей замены параметра соответствующей передаточной функции: 
$p \to 1/p$

## Пакет DSP.jl

https://docs.juliadsp.org/stable/filters/#DSP.Filters.Butterworth
