---
## Front matter
title: "Отчёт по лабораторной работе"
subtitle: "Лабораторная работа № 8"
author: "Живцова Анна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Освоить на практике применение режима однократного гаммирования на примере кодирования различных исходных текстов одним ключом.      

# Задание

Требуется разработать приложение, позволяющее шифровать и дешифровать данные в режиме однократного гаммирования.    

# Теоретическое введение

Предложенная Г. С. Вернамом так называемая «схема однократного использования (гаммирования)»  является простой, но надёжной схемой шифрования данных.    
Гаммирование представляет собой наложение (снятие) на открытые (зашифрованные) данные последовательности элементов других данных, полученной с помощью некоторого криптографического алгоритма, для получения зашифрованных (открытых) данных. Иными словами, наложение гаммы — это сложение её элементов с элементами открытого (закрытого)
текста по некоторому фиксированному модулю, значение которого представляет собой известную часть алгоритма шифрования.    
В соответствии с теорией криптоанализа, если в методе шифрования используется однократная вероятностная гамма (однократное гаммирование)
той же длины, что и подлежащий сокрытию текст, то текст нельзя раскрыть @crypt.

# Выполнение лабораторной работы

1. Создана программа, шифрующая сообщение по ключу.     

2. При шифровке двух текстов одним ключом, создана программа, определяющая один из исходных текстов по двум шифротекстам и  одному исходному тексту.        

3. Программа проверена на данных из пособия 

Листинг кода

```python 

coding_const = 1040 - 192
def one_in_rus(text1, text2):
    text1 = [ord(i) - coding_const if ord(i) > coding_const else 
                                          ord(i)  for i in text1]
    text2 = text2.split(' ')
    return  ' '.join(format(text1[i] ^ int(text2[i], 16), 'X') 
                                      for i in range(len(text1)))

def find_text_2(text1, crypt_text_1, crypt_text_2):
    text1 = [ord(i) - coding_const if ord(i) > coding_const else 
                                        ord(i) for i in text1]
    crypt_text_1 = crypt_text_1.split(' ')
    crypt_text_2 = crypt_text_2.split(' ')
    return  ''.join(chr((int(crypt_text_1[i], 16) ^ 
                int(crypt_text_2[i], 16) ^ text1[i]) + coding_const) 
    if int(crypt_text_1[i], 16) ^ int(crypt_text_2[i], 16) ^ text1[i] > 192
    else chr(int(crypt_text_1[i], 16) ^ int(crypt_text_2[i], 16) ^ text1[i]) 
                                               for i in range(len(text1)))
```

Процесс тестирования

```python
key = '05 0C 17 7F 0E 4E 37 D2 94 10 09 2E 22 57 FF C8 0B B2 70 54'
text1 = 'НаВашисходящийот1204'
text2 = 'ВСеверныйфилиалБанка'
crypt_text1 = one_in_rus(text1, key)
crypt_text2 = one_in_rus(text2, key)
find_text_2(text1, crypt_text1, crypt_text2)
```

Вывод ``` ВСеверныйфилиалБанка```

# Выводы

Успешно реализовали метод однократного гаммирования.

# Список литературы{.unnumbered}

:::{#refs}
:::   
