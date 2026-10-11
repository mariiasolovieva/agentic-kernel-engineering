# Seed literature

Стартовый список литературы. Агент может расширять его, но начинать с этих статей.

## SOAP и дескрипторы локального окружения
- Bartók, Kondor, Csányi (2013). "On representing chemical environments".
  О чём: определение SOAP, power spectrum, сравнение с другими дескрипторами.
  Зачем: базовая формула для кодирования окружения атома Li.
  PDF: data/papers/bartok2013_soap.pdf

- Behler (2011). "Atom-centered symmetry functions for constructing high-dimensional neural network potentials".
  О чём: симметрийные функции, инвариантность к поворотам и перестановкам.
  Зачем: понять, какие свойства дескриптора важны для GPR.
  PDF: data/papers/behler2011_acsf.pdf

## Set representation
- Xie et al. (2025). "Advances in Set Function Learning: A Survey of Techniques and Applications".
  О чём: как представить множество элементов вектором фиксированной длины.
  Зачем: выбор варианта пулинга для разного числа интеркалированных атомов в матрице.
  PDF: data/papers/xie2025_representation.pdf

- Corso et al. (2020). "Principal Neighbourhood Aggregation for Graph Nets".
  О чём: применение статистического пулинга.
  Зачем: конкретный пример представления множества вектором фиксированной длины - статистический пулинг.
  PDF: data/papers/corso2020_stats.pdf

## GPR и BO
- Mattias Seeger (2004). "Gaussian Processes for Machine Learning". 
  О чём: теория GPR, ядра, маргинальное правдоподобие.
  Зачем: основа для выбора ядра и режима обучения.
  PDF: data/papers/seeger2004_gpr_basics.pdf

- Shahriari et al. (2016). "Taking the human out of the loop: A review of Bayesian optimization". 
  О чём: обзор BO, acquisition functions, применение.
  Зачем: понять, как встроить GPR в цикл BO.
  PDF: data/papers/shahriari2016_bo_basics.pdf

## Подбор ядра
- Duvenaud et al. (2013). "Structure discovery in nonparametric regression through compositional kernel search".
  О чём: поиск структуры ядра как композиции базовых.
  Зачем: подзадача выбора комбинации ядер.
  PDF: duvenaud13_composition_kernel.pdf

- Shuaib et al. (2025). "Multikernel similarity-based clustering of amorphoussystems and machine-learned interatomic potentials byactive learning".
  О чём: гибридное ядро подобия, которое демонстрирует интеграцию дескрипторов ближнего и дальнего действия с помощью метода усредненного ядра.
  Зачем: приложение similarity kernel к химическим системам; статья похожа на мою идею для аккумуляторов, но реализованная на других системах.
  PDF: shuaib2024_multikernel_amorphous.pdf

- Lei et al. (2016). "Localized Multiple Kernel Learning—A Convex Approach".
  О чём: выпуклый подход к обучению нескольких ядер
  Зачем: описание варианта решения задачи подбора параметров ядра как выпуклой задачи. Пригодится для второго проекта по этой теме, но для курса "Выпуклый анализ и оптимизация".
  PDF: lei2016_multiple_kernel.pdf