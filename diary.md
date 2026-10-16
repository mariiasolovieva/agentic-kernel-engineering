* Я писала на английском языке файлы, которыми пользуется агент, для экономии токенов, а дневник - на русском для себя. Этот файл чисто для учебных целей

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

## Задача 2: сжатие статьи

Я хочу сжать статью data/papers/shuaib2024_multikernel_amorphous.pdf - это прикладная статья в области материаловедения с обширной математической базой, то, к чему я стремлюсь в своей работе.

### Запрос 2.1

Read data/papers/shuaib2024_multikernel_amorphous.pdf.
Create three levels of the text compression:
- memory/L1.md — section-by-section summary (Abstract, Introduction,
  Computational methods, Models, Methodology, Problem statement).
- memory/L2.md — 5–10 key claims, one sentence each.
- memory/L3.md — one sentence for the whole paper.
For at least 3 claims in L2, add a verification line:
  [verified: p. X, "short quote"]
Print 'Compression on the levels L1/L2/L3 done'.
Do not start anything else.

### Результат 2.1

L1.md
- я не указала в запросе при перечислении разделов "результаты" и "обсуждение", и агент, очень четко следуя инструкции, практически не упомянул информацию из результатов. При этом он сделал саммари выводов, но назвал это "результаты и выводы".

L2.md
- проверка всех цитирований показала, что агент не допустил ни одной ошибки/галлюцинации, все страницы верны, нет вымышленных утверждений. Единственный недочет - форматирование надстрочного индекса в аббривеатуре MLIPA(1) вместо MLIP^A(1).

L3.md
- агент справился с задачей "сжать статью в одно предложение", но предложение получилось в стиле Льва Толстого (очень длинное и сложное, много подробностей/конкретных значений, без чего скорее всего можно было бы обойтись в данной задаче).

### Точка проверки 2.1

Все цитирования соответствуют действительности, галлюцинаций и придуманных фактов нет (проверка выполенена по явным указаниям страниц и цитат в L2.md и по общей логике для L1.md и L3.md)

### Итог 2.1

Сжатие получилось без галлюцинаций, в следующем запросе поправим отсутствие разделов Results и Discussion

### Запрос 2.2

Read memory/L1.md and data/papers/shuaib2024_multikernel_amorphous.pdf.
The current L1.md is missing the "Results" and "Discussion" sections.
Add two new subsections to L1.md:
- "Results": summarize Section 3 (Clustering, AL MLIP, MLIP-MD,
  structure factors, PDFs, coordination numbers).
- "Discussion and Conclusion": summarize Section 4.
For each new subsection, give 2–4 sentences. Keep the same style
as the existing sections. Do not rewrite the other sections.
After editing, print 'Resutls and Discussion sections compression done' and stop.

### Запрос 2.3

Also check memory/L2.md: does it include any claims from the Results
or Conclusion sections? If not, add 1–2 claims with
[verified: p. X, "quote"] lines.
And rewrite memory/L3.md: the current sentence is too long.
Aim for one clear sentence without specific numerical values.

### Результат 2.2-2.3

Агент добавил два блока ("Results" и "Discussion and Conclusions"), в дополнение к уже существующему "Results".

### Точка проверки 2.2-2.3

По содержанию вопросов нет (как и в предыдущих пунктах), но есть некоторая путаница в именовании разделов. 

### Итог 2.2-2.3

По сути, я не указала что нужно модифицировать существующий раздел "Results", так что вопросов к нему нет, только к формулировкам в моих запросах. Содержательно, необходимая задача выполнена. 

## Задача 3: генерация файла graph.mmd по ontology.yaml

Я плохо знакома с данным форматом, поэтому я самостоятельно составила онтологию проекта, а graph.mmd доверила сгенерировать агенту.

### Запрос 3

Read AGENTS.md, INDEX.md, and ontology.yaml.
Generate graph.mmd: a Mermaid diagram of the concept graph from ontology.yaml.
- Use classes as nodes and relations as labeled edges.
- Include the instances as leaf nodes, connected to their classes
  via dashed edges labeled "instance_of".
- Keep it readable: group by domain if helpful.
Write the file to graph.mmd in the project root. Then print 'graph.mmd generated' and stop.

### Результат 3

Агент создал flowchart TB с 14 классами, 12 связями и 24 экземплярами.

### Точка проверки 3

Сравнила списки классов/связей/экземпляров в graph.mmd и ontology.yaml — совпадают; проверила 3 правила со слайда 73 лекции недели 3, все выполнены.

### Итог 3 

Граф корректен, онтология и мини-граф готовы.

## Задача 4: запросы к локальному контексту

### Запрос 4.1

Find the formula for the average SOAP kernel in shuaib2024

### Результат 4.1

$$K_{SOAP}(A,B) = \frac{1}{N_A \times N_B} \sum_{ij} C_{ij}(A,B) = \left[ \ell \left( \frac{1}{N_A} \sum_i P(\epsilon_i^A) \right)^{\top} \cdot \left( \frac{1}{N_B} \sum_j P(\epsilon_j^B) \right) + c_0 \right]^d$$

where:
- $C_{ij}(A,B) = k(\epsilon_i^A, \epsilon_j^B)$ is the pairwise SOAP similarity between atom $i$ in structure $A$ and atom $j$ in structure $B$ (Eq. 8),
- $N_A$, $N_B$ are the atom counts of the two configurations,
- $P(\epsilon_i^A)$ is the SOAP power-spectrum vector of the local environment of atom $i$,
- $\ell$, $c_0$, $d$ are the polynomial-kernel parameters ($d=4$ in this work),
- The computation is cheap because it only requires retaining the **average SOAP fingerprint** $\frac{1}{N_A}\sum_i P(\epsilon_i^A)$ across all atom environments in each structure.

This is the same formula already recorded in `memory/L1.md` (Models/Methodology section, Eq. 9).

### Запрос 4.2

Find in shahriari2016_bo_basics.pdf the comparison of EI and UCB. Write down pros and cons for each acquisition function and choose a more appropriate for our work_plan.md.

### Результат 4.2

The work_plan.md now has an explicit pros/cons breakdown for EI and UCB, verified against the Shahriari paper (p.13–15), plus the recommendation.

## Summary

**EI (Expected Improvement)** — Shahriari p.14, Eq. 44:
- **Pros:** incorporates the *amount* of improvement (not just probability); works well with the adaptive target τ = best observed value; convergence rates proven.
- **Cons:** no provable regret bounds; requires choosing target τ.

**UCB (Upper Confidence Bound)** — Shahriari p.14, Eq. 45:
- **Pros:** provable cumulative regret bounds (GP-UCB); theoretically motivated guidelines for setting/scheduling β_n.
- **Cons:** performance depends on tuning β_n; no explicit improvement target, less intuitive.

**Choice for our work_plan:** **EI** as the primary acquisition function — the paper explicitly states it works well with the adaptive best-observed target, whereas PI is overly greedy. UCB is kept as a valid alternative with provable regret bounds; we run both and compare convergence empirically (the paper warns "no single acquisition strategy provides better performance over all problem instances", p.15).

I added the explicit pros/cons block to `memory/work_plan.md` §2.5.2 (all quotes verified against the paper). The recommendation was already present and is now backed by the clearer breakdown.

В context_queries.md я записала еще три примера запросов к локальному контексту, но не стала их прогонять, поскольку в целом мы разобрали новую тему, в которой я только начинаю разбираться - задача была решена.