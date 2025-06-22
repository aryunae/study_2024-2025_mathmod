---
## Front matter
lang: ru-RU
title: Лабораторная работа №6
subtitle: Задача об эпидемии
author:
  - Ендонова А.В.
institute:
  - Российский университет дружбы народов, Москва, Россия

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Ендонова Арюна Валерьевна
  * студентка
  * Российский университет дружбы народов
  * [1132221888@pfur.ru](mailto:1132221888@pfur.ru)
  * <https://github.com/aryunae>

:::
::: {.column width="25%"}

![](./image/endonova.jpg)

:::
::::::::::::::


## Цель работы

Исследовать модель SIR (задача об эпидемии)

## Задание

На одном острове вспыхнула эпидемия. Известно, что из всех проживающих
на острове ($N=10900$) в момент начала эпидемии ($t=0$) число заболевших людей
(являющихся распространителями инфекции) $I(0)=210$, А число здоровых людей с
иммунитетом к болезни $R(0)=43$. Таким образом, число людей восприимчивых к
болезни, но пока здоровых, в начальный момент времени $S(0)=N-I(0)- R(0)$.

Постройте графики изменения числа особей в каждой из трех групп.

Рассмотрите, как будет протекать эпидемия в случае:

1) если $I(0)\leq I^*$;

2) если $I(0) > I^*$.

# Случай $I(0)\leq I^*$

## Реализация на Julia

```Julia

function sir_2(u,p,t)
    (S,I,R) = u
    (b, c) = p
    N = S+I+R
    dS = 0
    dI = -c*I
    dR = c*I
    return [dS, dI, dR]
end
```

## Реализация на Julia

``` Julia
N = 10900
I_0 = 210
R_0 = 43
S_0 = N - I_0 - R_0
u0 = [S_0, I_0, R_0]
p = [0.1, 0.05]
tspan = (0.0, 200.0)
```

## Реализация на Julia

```Julia
prob_2 = ODEProblem(sir_2, u0, tspan, p)
sol_2 = solve(prob_2, Tsit5(), saveat = 0.1)
plot(sol, label = ["S" "I" "R"])
```

## Реализация на Julia

![Динамика изменения числа людей в каждой из трех групп](image/sir_2.png){#fig:001 width=70%}

## Реализация на OpenModelica

```
  parameter Real I_0 = 210;
  parameter Real R_0 = 43;
  parameter Real S_0 = 10647;
  parameter Real N = 10900;
  parameter Real b = 0.1;
  parameter Real c = 0.05;
  Real S(start=S_0);
  Real I(start=I_0);
  Real R(start=R_0);
equation
  der(S) = 0;
  der(I) = - c*I;
  der(R) = c*I;
```

## Реализация на OpenModelica

![Динамика изменения числа людей в каждой из трех групп](image/sir_2_OM.png){#fig:002 width=80%}

# Случай $I(0) > I^*$

## Реализация на Julia

```Julia

function sir(u,p,t)
    (S,I,R) = u
    (b, c) = p
    N = S+I+R
    dS = -(b*S*I)/N
    dI = (b*I*S)/N - c*I
    dR = c*I
    return [dS, dI, dR]
end
```

## Реализация на Julia

``` Julia
N = 10900
I_0 = 210
R_0 = 43
S_0 = N - I_0 - R_0
u0 = [S_0, I_0, R_0]
p = [0.1, 0.05]
tspan = (0.0, 200.0)
```

## Реализация на Julia

```Julia
prob = ODEProblem(sir, u0, tspan, p)
sol = solve(prob, Tsit5(), saveat = 0.1)
plot(sol, label = ["S" "I" "R"])
```

## Реализация на Julia

![Динамика изменения числа людей в каждой из трех групп](image/sir_1.png){#fig:003 width=70%}

## Реализация на OpenModelica

```
  parameter Real I_0 = 210;
  parameter Real R_0 = 43;
  parameter Real S_0 = 10647;
  parameter Real N = 10900;
  parameter Real b = 0.1;
  parameter Real c = 0.05;
  Real S(start=S_0);
  Real I(start=I_0);
  Real R(start=R_0);
equation
  der(S) = -(b*S*I)/N;
  der(I) = (b*I*S)/N - c*I;
  der(R) = c*I;
```

## Реализация на OpenModelica

![Динамика изменения числа людей в каждой из трех групп](image/sir_1_OM.png){#fig:004 width=80%}

## Выводы

В результате выполнения данной лабораторной работы я исследовала модель SIR.

## Список литературы

1. Compartmental models in epidemiology [Электронный ресурс]. URL: https: //en.wikipedia.org/wiki/Compartmental_models_in_epidemiology.
