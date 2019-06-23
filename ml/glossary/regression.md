# Регрессионные модели  

## Linear Regression
Линейная регрессия - метод восстановления зависимости между двумя переменными  
Суть метода заключается в подборе векторов коэффициентов W для уравнения: Y = WX.  

Для использования линейной регрессии необходимо соблюдение двух обязательных условий:
 - постоянная дисперсия для всей выборки  
 - отсутствие автокорреляции случайных ошибок  
для оценки качества модели стоит использовать **SSE**  
Также стоит проверять значение числа обусловленности для диагностирования линейной зависимости
между аргументами  

За счет простой формы быстро и легко обучаются, популярны при работе с большими объемами данных  
Для реальных прогнозов и упреждающего анализа линейная регрессия не очень подходит  
Может очень хорошо работать на тренировочных данных и плохо на тестовых, т.е. модели линейной регрессии склонны к переобучению  

Коэффициенты W подбираются по формуле:
```python
W = (Xt*X)^-1 * Xt*y
          (1 x1)
, где X = (1 x2) (это одна матрица)
          (1 x3)
Xt - транспонированная
X^-1 - обратная матрица
```

## KNN (K nearest neighbors)
Метод k-средних - алгоритм используемый как для классификации, так и для регрессии  
В случае классификации объекту будет назначен класс как у k ближайших соседей  
В случае же регрессии объекту присваивается среднее значение k ближайших соседей  
Для данного алгоритма обычно необходимо проводить нормировку данных, поскольку используется
дистанция, а сильно отличающиеся по порядкам параметры могут начать сильно влиять на результат  
Применение:
 - часто используется для построения meta-признаков  
 - рекомендательные системы  

Преимущества:
 - простая реализация  
 - можно гибко использовать, задавая кастомные метрики  
 - неплохая интерпретация  

Недостатки
 - на больших объемах может работать довольно медленно  
 - при большом количестве признаков сложно отобрать значимые  
 - нет теоретических оснований выбора определенного числа соседей — только перебор  
 - в случае малого числа соседей метод чувствителен к выбросам, то есть склонен переобучаться  
 - как правило, плохо работает, когда признаков много, из-за "прояклятия размерности"  


## Decision Tree
В принципе, можно использовать деревья для построения регрессионных моделей  
[см decision trees](./decision_trees.md)


## Lasso regression
(L1-регуляризация)
Cходна с гребневой, за исключением того, что коэффициенты регрессии могут равняться нулю (часть признаков при этом исключается из модели)  


## Ridge regression(гребневая)
(L2-регуляризация)
Усовершенствование линейной регрессии с повышенной устойчивостью к ошибкам,
налагающая ограничения на коэффициенты регрессии для получения куда более
приближенного к реальности результата.  

В отличие от L1-регуляризации добавляется penalty (наказание) к функции потерь  
Часто применяется для понижения размерности, поскольку позволяет избавиться от наименее значимых аргументов.  
Когда стоит использовать:
 - при сильной обусловленности признаков  
 - сильно различаются собственные значения или некоторые из них близки к нулю  
 - в аргументах есть сильно скоррелированные значения  

Ридж-регрессия или гребневая регрессия предполагает оценку параметров по следующей формуле:
```python
W = (Xt*X + LI)^-1 * Xt*y
          (1 x1)
, где X = (1 x2) (это одна матрица)
          (1 x3)
Xt - транспонированная
X^-1 - обратная матрица
L - это коэффициент регуляризации, то, насколько сильно мы хотим учитывать условие I
```

## Байесовская регрессия
Похожа на гребневую регрессию, однако основана на том допущении,
что в данных шум (ошибка) распределен нормально  

## Links
[10 типов регрессии](http://datareview.info/article/10-tipov-regressii-kakoy-vyibrat/)
[Базовые принципы машинного обучения на примере линейной регрессии](https://habr.com/ru/company/ods/blog/322076/)