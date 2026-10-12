## Задача 1: составление плана работы

Мной была сформулирована исследовательская задача и конкретные вопросы, которые меня интересовали по теме (в которой я пока не являюсь хорошим специалистом). Я отобрала литературу по каждому из пунктов/подзадач, которые я выделила самостоятельно (data/papers) и кратко описала их содержание в data/papers/seed.md. Самостоятельно я читала только абстракты и кратко просматривала статьи на предмет соответствия подзадачам (например: подзадача - закодировать структуру материала через дескриптор SOAP, о котором я знаю только, что он находит применение во многих графовых нейросетях для молекул; статья для агента - обзор, как устроен дескриптор SOAP. Точно также для методов Bayesian optimization и Gaussian process regression).

### Запрос 1.1

Read AGENTS.md and literature/seed.md.
Propose a work plan for the BO&GPR project on Li intercalation:
1) how to obtain a fixed-length feature vector based on SOAP
   for structures with different numbers of intercalated atoms;
2) which kernel selection experiments to conduct locally;
3) which tables and plots should be produced as output;
4) how to verify each step without a cluster.
For each step, provide justification from the literature (page and quote).

### Результат 1.1

Агент создал файл memory/work_plan.md.

Комментарии по содержанию плана:
- агент проверил, установлены ли у меня необходимые для работы пакеты (такие как dscribe, sklearn, gputorch) и предлагает на этапе 0 создать виртуальную среду venv и установить их туда;
- агент предложил на основе анализа литературы другой вариант пулинга (изначально я просила проверить статистический пулинг векторов, на основе прочитанных статей из data/papers агент предлагает использовать average SOAP fingerprint), даны ссылки на конкретные утверждения в статьях, подтверждающие такой вывод;
- агент предложил адекватный план для экспериментов для выбора ядра (у меня есть прототип данного проекта без использования SOAP-дескрипторов, и в части тестирования ядер я придерживалась примерно такой же логики, что и агент - т.е. на данном этапе можно сказать, что план агента верифицирован);
- агент как будто забыл изучить часть про Байесовскую оптимизацию (последний этап сборки проекта). Он включил БО в список таблиц и графиков, которые мы должны получить в конце работы, но не предложил план реализации BO-loop с опорой на литературу (хотя она была в папке data/papers/ для изучения). Будем это исправлять на следующем шаге.

### Точка проверки 1.1

Я самостоятельно проверила все точки "Justification", которые привел агент в work_plan.md. Никаких галлюцинаций не обнаружено, цитаты точные, страницы правильные, в уравнениях также нет ошибок.

### Итог 1.1

Агент написал хороший и адекватный план работы, но забыл один из пунктов. Будем дорабатывать это в следующем запросе. 

Также уточнила в AGENTS.md инструкцию по работе с данными:
- `data/structures/` — example structures from the training set (in POSCAR format). All input data (POSCAR files, .csv training sets) is provided by me.
  Do not generate synthetic structures, do not create toy datasets, do not fabricate any data. If a step requires data that is not in
  data/structures/ yet, ask me to provide it.- `data/structures/` — example structures from the training set (in POSCAR format). All input data (POSCAR files, .csv training sets) is provided by me.
  Do not generate synthetic structures, do not create toy datasets, do not fabricate any data. If a step requires data that is not in
  data/structures/ yet, ask me to provide it.

и роль агента в проекте:
- Your role: literature analysis, code writing, verification of results.
  No data generation, no running cluster calculations.


### Запрос 1.2

I read memory/work_plan.md. The plan is good overall, but one section
is missing: Bayesian optimization.

Task:
Add a new section "Bayesian optimization loop" to memory/work_plan.md,
placed after "Kernel selection experiments".

In this section, justify from the literature (page and quote for each claim):
1) how to construct the BO loop on top of the trained GPR surrogate;
2) which acquisition function to use and why (EI, UCB, PI — compare
   at least two options using data/papers/shahriari2016_bo_basics.pdf);
3) how to initialize the BO loop (initial design, number of points);
4) what the stopping criterion should be for our local, cluster-free setup;
5) what outputs the BO loop should produce (a table of queried
   configurations with predicted mean and variance, a convergence plot,
   an artifact with the best configuration found).

For every claim, give the source: PDF filename, page number, and a short
quote. If a claim is not supported by the papers in data/papers/, say so
explicitly instead of guessing. After editing, print 'Literature analysis done', then stop and wait for my next request. Do not start implementing anything.

### Результат 1.2

Агент добавил раздел по анализу литературы на тему Bayesian optimization. Выбрана немного странная нумерация (2.5 после просто пункта 2) - но это абсолютно не критично. Также агент учел обновления AGENTS.md и все требования в промпте.

### Точка проверки 1.2

На данном этапе такая же проверка - по общим соображениям и по цитированию утверждений и формул/уравнений. Все цитирования соответствуют источникам, что просто отлично.

### Итог 1.2

Теперь план работы полностью укомплектован. 