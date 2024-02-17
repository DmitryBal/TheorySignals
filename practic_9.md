# Практика 9 

## Задание

1. По [[lecture_9.pdf]] разобраться, чем отличаются рассмотренные там два метода проектирования дискретного БИХ-фильтра по аналоговому фильтру прототипу. Выяснить следующие вопросы:

- как в каждом из методов осуществляется отображение p-плоскости в z-плоскость, какие имеются особенности у этих отображений;
- визуализировать эти отображения, воспользовавшись соответствующим программным кодом из [[lecture_9]] (или написать свой аналогичный код).
  
2. Выполнить следующие, предлагаемые ниже задания, предварительно изучив необходимые для этого функции из библиотеки DSP.jl (или аналогичные функции из какой-либо другой библиотеки).
    
    
    2.1. Спроектировать аналоговый фильтр Баттерворта нижних частот некоторого порядка (4...6) с частотой среза, равной 1 (нормированный фильтр), т.е. получить коэффициенты его передаточной функции.
    
    2.2. Преобразовать спроектированный в предыдущем пункте аналоговый фильтр к следующим фильтрам
    - к фильтру нижних частот с частотой среза $f_H=10 КГц$, выполнив замену $p \to p/(2\pi f_H)$;
    - к фильтру верхних частот с частотой среза $h_L=20 KГц$, выполнив замену $p \to (2\pi f_L)/p$;
    - к полосовому фильтру с полосой пропускания от $f_L=10 КГц$ до $f_H=20 КГц$, выполнив замену $p \to \Big (p/(2\pi f_0)+(2\pi f_0)/p \Big)\frac{f_0}{f_H-f_L}$, где $f_0=\sqrt{f_Lf_H}$ - средняя частота;
      
	2.3. Спроектировать аналоговый фильтр Чебышева 1-го рода нижних частот с частотой срез, равной 1 (нормированный фильтр) и с неравномерностью AЧХ в полосе пропускания 1 дБ (децибел).
	
	2.4. Выполнить пункты задания 2.2, но применительно к спроектированному в предыдущем пункте аналоговому НЧ-фильтру Чебышева 1-го рода. 
	
	2.5. Спроектировать цифровые фильтры методом билинейного преобразования по аналоговым фильтрам прототипам, спроектированным в пунктах 2.2 и 2.4. 
	
	2.6. Объяснить, почему метод инвариантной импульсной характеристики абсолютно не годится для проектирования цифрового фильтра верхних частот (в качестве прототипа здесь предполагается соответствующий аналоговый фильтр высоких частот). Как будет проявляться эффект наложения в этом случае?

3. Построить графики AЧХ для всех спроектированных в предыдущих пунктах аналоговых и цифровых фильтров.

4. Построить карты нулей и полюсов для каждой рассматриваемой пары аналогового и цифрового фильтров.

5. Для цифрового НЧ-фильтра Баттерворта из пункта 2.2 составить каноническую структурную схему (это надо сделать вручную).
   
6. Пропустить через этот НЧ-фильтр (с частотой среза $10 Кгц$) дискретный сигнал, получаемый в результате дискретизации сигнала, представляющего собой сумму двух гармонических сигналов $x(t)=A\cos(2\pi f_1 t) + A \cos(2\pi f_2 t)$, где $f_1=5КГц, f_2=25КГц$, амплитуду $A$ можно считать равной 1. Период дискретизации требуется выбрать самостоятельно. Для этого воспользоваться специальной библиотечной функцией (обычно она называется filter), которая осуществляет вычисления в соответствии со структурной схемой, полученной в предыдущем пункте. При этом необходимо
    - уяснить назначение параметра этой функции, через который в эту функцию можно передавать вектор состояния системы (см. структурную схему, которая включает в себя переменные состояния); по умолчанию этот вектор состояния состоит из одних нулей.
    - построить два графика (один под другим) представляющих входной и выходной дискретные сигналы (графики эти должны быть без интерполяции); убедиться, что отсчеты выходного сигнала на этом графике появляются с некоторой задержкой по отношению к отсчетам входного сигнала; объяснить этот эффект с использованием структурной схемы.
   
   
Все пункты задания выполнить, воспользовавшись какой-либо библиотекой функций по цифровой обработке сигналов. Например, можно воспользоваться пакетом [DSP.jl](https://docs.juliadsp.org/stable/filters/#Filters-filter-design-and-filtering) (а также подсказками чата GPT).

## Приложение. Краткое описание некоторых функций пакета DSP.jl

### Пример проектирования дискретного БИХ-фильтра
```julia
#Фильтруйте данные в `x`, отобранные с частотой 1000 Гц, с помощью #полосового фильтра Баттерворта 4-го порядка в диапазоне от 10 до 40 Гц:
using DSP 
#= 1. Создается объект типа Butterworth, определяющий нормированный НЧ-фильтр баттерворта 4-го порядка:
=#
designmethod = Butterworth(4)

#= 2. Создаётся объект типа Bandpass, определяющий полосовой фильтр (не важно какго типа) с заданными граничными частотами (f_L=10 Гц и f_H=40 Гц) и с зал\данной частотой дискретизации (fs=1000 период дискретизации $\tau=1/fs$): 
=#
responsetype = Bandpass(10, 40; fs=1000)

#= 3. На основе аналогового фильтра-прототипа (с определеными выше характеристиками) создается соответствующий дискретный (цифровой) фильтр:
=#
digital_filter = digitalfilter(responsetype, designmethod)

#= Теперь, если только предварительно был сформированн вектор x, содержащий отсчеты входного сигнала, то вектор с отсчетами выходного сигнала получается с помощью специальной функции filt:
=#
y = filt(digital_filter, x)
```

### Функции для проектирования дискретных фильтров с различными областями пропускания/задержания
Для проектирования дискретных (цифровых) НЧ- и ВЧ-фильтров с заданной частотой среза, полосовых и заградительного (режекторного) фильтров фильтров с заданной полосой пропускания/задержания в пакете DSP.jl имеются следующие конструкторы.

Конструктор дискретного фильтра нижних частот [`DSP.Filters.Highpass`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Lowpass)
```julia
Lowpass(Wn[; fs])
```

Конструктор дискретного фильтра верхних частот [`DSP.Filters.Highpass`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Lowpass)
```julia
Highpass(Wn[; fs])
```

Конструктор дискретного полосового фильтра [`DSP.Filters.Bandpass`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Bandpass)
```julia
Bandpass(Wn1, Wn2[; fs])
```

Конструктор дискретного заградительного фильтра[`DSP.Filters.Bandstop`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Bandstop)
```julia
Bandstop(Wn1, Wn2[; fs])
```

Во всех случаях `Wn,Wn1,Wn2` - это частоты среза; при этом, если частота дискретизации `fs` не указана, то частоты среза интерпретируется как нормированные частоты (т.е. частоты, отнесенные к частоте Найквиста, которая равна `fs/2`).
### Функции для проектирования аналоговых нормированных НЧ-фильтров-прототипов

Конструктор нормированного НЧ-фильтра Баттерворта n-го порядка [`DSP.Filters.Butterworth`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Butterworth)```julia
Butterworth(n)
```

Конструктор нормированного НЧ-фильтра Чебышева 1-рода n-го порядка и с заданным в дБ уровнем пульсаций в полосе пропускания riple [`DSP.Filters.Chebyshev1`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Chebyshev1)
```julia
Chebyshev1(n, ripple)
```

Конструктор нормированного НЧ-фильтра Чебышева 2-рода n-го порядка и с заданным в дБ уровнем пульсаций в полосе пропускания riple 
[`DSP.Filters.Chebyshev2`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Chebyshev2)
```julia
Chebyshev2(n, ripple)
```

Конструктор нормированного НЧ-фильтра эллиптического типа (Кауэра) n-го порядка и с заданным в дБ уровнем пульса
ций в полосе пропускания rp и в полосе задержания rs
[`DSP.Filters.Elliptic`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.Elliptic)
```julia
Elliptic(n, rp, rs)
```

### Функции для проектирования передаточных функций аналоговых и цифровых фильтров

Функция[`DSP.Filters.digitalfilter`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.digitalfilter)
```julia
digitalfilter(responsetype, designmethod)
```
Эта функция возвращает значение типа ZeroPoleGain{:z}`, т.е. это значение представляет собой структуру, содержащую массивы нулей и полюсов, а также коэффициен усиления передаточной функции соответствующего цифрового фильтра. 

Функция [`DSP.Filters.analogfilter`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.analogfilter)
```julia
analogfilter(responsetype, designmethod)
```

Эта функция возвращает значение типа {:s}, т.е. это значение представляет собой структуру, содержащую массивы нулей и полюсов, а также коэффициентов усиления передаточной функции соответствующего аналогового фильтра.

Чтобы преобразовать представление фильтра в виде структуры См.[`DSP.Filters.ZeroPoleGain`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.ZeroPoleGain) к представлению в виде структуры [`DSP.Filters.PolynomialRatio`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.PolynomialRatio), содержащей массивы с коэффициентами многочленов числителя и знаменателя передаточной функции, надо воспользоваться функцией `convert` (эта функция имеется в языке Julia). После такого преобразования можно будет воспользоваться функциями[`DSP.Filters.coefb`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.coefb), и [`DSP.Filters.coefa`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.coefb) возвращающими коэффициенты числителя и знаменателя передаточной функции, соответственно.

В пакете имеются также функции fracs (для получения значений передаточной функции аналогового фильтра), fraqz (для получения значений передаточной функции цифрового фильтра), unwrap (для устранения разрывов при построении графика фазовой характеристики), grpdelay (для вычисления времени группового запаздывания (ГВЗ) в зависимости от частоты). Для получения фазо-частотной характеристики (ФЧХ) фильтра (аналогового или цифрового) можно использовать имеющуюся непосредственно в языке Julia функцию angle, возвращающую аргумент комплексного числа.

Пример. https://docs.juliaplots.org/latest/tutorial/
```julia
using DSP, Plots

desig_nmethod = Butterworth(4)
response_type = Bandpass(10, 40; fs=1000)
digital_filter = digitalfilter(response_type, design_method)
frequency_response, ww = freqz(digital_filter)
phase_response = unwrap(angle.(frequency_response))
grp_delay = grpdelay(frequency_response)

p1=plot(plt, ww, abs.(frequency_response))
p2=plot(plt, ww, phaze_response)
p3=plot(plt, ww, grp_delay)
plot(p1,p2,p3, layout = grid(3,1), title=["AЧХ" "ФЧХ" "ГВЗ"])
```

### Функции для определения требуемых параметров фильтра, включая его порядок 

На практике требования по частотной избирательности фильтра часто задаются в виде требований ослабления на заданное число дБ на границе (границах) частотных диапазонов пропускания/прозрачности.

Например, применительно к НЧ-фильтру может быть указано, что на частотах не превышающих Wp ослабление должно быть не более чем Rp дБ, а на частотах выше Ws ослабление должно быть не менее Rs дБ.

В этом случае возникает задача выбора параметров фильтра определенного типа (Баттерворта, Чебышева и т.п.), обеспечивающих заданное требование при наименьшем возможном порядке фильтра.  

Такая постановка задачи касается как аналоговых, так и цифровых фильтров, а также НЧ-, ВЧ-фильтров, полосовых и заградительных фильтров.

Для решения этой задачи в пакет DSP.jl имеются следующие функции, каждая из которых реализована в двух вариантах. Один вариант относится к случаю НЧ- или ВЧ-фильтра, а другой - к случаю полосового или заградительного фильтра.

Функция [`DSP.Filters.buttord`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.buttord)

```julia
(N, ωn) = buttord(Wp::Tuple{Real,Real}, Ws::Tuple{Real,Real}, Rp::Real, Rs::Real; domain=:z)
```
```julia
(N, ωn) = buttord(Wp::Real, Ws::Real, Rp::Real, Rs::Real; domain=:z)
```

Функция [`DSP.Filters.cheb1ord`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.cheb1ord)

```julia
(N, ωn) = cheb1ord(Wp::Real, Ws::Real, Rp::Real, Rs::Real; domain::Symbol=:z)
```
```julia
(N, ωn) = cheb1ord(Wp::Tuple{Real, Real}, Ws::Tuple{Real, Real}, Rp::Real, Rs::Real; domain::Symbol=:z)
```

Функция [`DSP.Filters.cheb2ord`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.cheb2ord)
```julia
(N, ωn) = cheb2ord(Wp::Real, Ws::Real, Rp::Real, Rs::Real; domain::Symbol=:z)
```
```julia
(N, ωn) = cheb2ord(Wp::Tuple{Real, Real}, Ws::Tuple{Real, Real}, Rp::Real, Rs::Real; domain::Symbol=:z)
```

Функция [`DSP.Filters.ellipord`](https://docs.juliadsp.org/stable/filters/#DSP.Filters.ellipord)

```julia
(N, ωn) = ellipord(Wp::Real, Ws::Real, Rp::Real, Rs::Real; domain::Symbol=:z)
```
```julia
(N, ωn) = ellipord(Wp::Tuple{Real, Real}, Ws::Tuple{Real, Real}, Rp::Real, Rs::Real; domain::Symbol=:z)
```

Для получения более подробной информации по каждой из функций следует перейти по соответствующей ссылке.