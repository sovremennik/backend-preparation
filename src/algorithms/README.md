# Алгоритмы

## 1. Введение в алгоритмы
- Определение и классификация  
- Оценка сложности алгоритмов (Big O, Big Ω, Big Θ)  
- Анализ времени и памяти  

## 2. Алгоритмы сортировки
### 2.1. Базовые сортировки
- Пузырьковая сортировка (Bubble Sort)  
- Сортировка вставками (Insertion Sort)  
- Сортировка выбором (Selection Sort)  

### 2.2. Эффективные сортировки
- Быстрая сортировка (Quick Sort)  
- Сортировка слиянием (Merge Sort)  
- Пирамидальная сортировка (Heap Sort)  

### 2.3. Специализированные сортировки
- Поразрядная сортировка (Radix Sort)  
- Сортировка подсчётом (Counting Sort)  
- Блочная сортировка (Bucket Sort)  

## 3. Поиск в данных
### 3.1. Линейный поиск (Linear Search)  
### 3.2. Бинарный поиск (Binary Search)  
- Итеративный и рекурсивный подход  
- Поиск в отсортированных структурах  

### 3.3. Интерполяционный и экспоненциальный поиск  

## 4. Жадные алгоритмы (Greedy Algorithms)
- Принцип работы  
- Задача о рюкзаке (Knapsack Problem)  
- Задача о покрытии множества (Set Cover Problem)  
- Задача о размене монет  

## 5. Динамическое программирование (Dynamic Programming)
- Оптимизация перекрывающихся подзадач  
- Задача о рюкзаке (0/1 Knapsack)  
- Задача о наибольшей возрастающей подпоследовательности  
- Задача о наибольшей общей подпоследовательности (LCS)  

## 6. Алгоритмы на графах
### 6.1. Обходы графов
- Поиск в глубину (DFS)  
- Поиск в ширину (BFS)  

### 6.2. Алгоритмы кратчайшего пути
- Алгоритм Дейкстры  
- Алгоритм Беллмана-Форда  
- Алгоритм Флойда-Уоршелла  
- A* (A-star)  

### 6.3. Алгоритмы поиска минимального остовного дерева (MST)
- Алгоритм Краскала  
- Алгоритм Прима  

### 6.4. Поиск сильных компонент (Kosaraju, Tarjan)  

## 7. Разделяй и властвуй (Divide and Conquer)
- Быстрая сортировка  
- Сортировка слиянием  
- Поиск ближайшей пары точек  

## 8. Алгоритмы "Два указателя" и "Скользящее окно"
- Поиск подстроки  
- Максимальная сумма подпоследовательности  

## 9. Алгоритмы работы со строками
- Поиск подстроки: KMP, Рабина-Карпа  
- Z-функция и префикс-функция  
- Алгоритм Боера-Мура  

## 10. Алгоритмы на деревьях
- Обходы дерева (Pre-order, In-order, Post-order)  
- Lowest Common Ancestor (LCA)  
- Дерево отрезков (Segment Tree)  
- Дерево Фенвика (Binary Indexed Tree)  

## 11. Рекурсия и итерация
- Принципы рекурсии  
- Хвостовая рекурсия  
- Мемоизация  

## 12. Алгоритмы на множествах и картах
- Хеширование  
- Сканирующая линия (Sweep Line Algorithm)  
- Алгоритмы с использованием `HashMap` и `HashSet`  

## 13. Вероятностные и рандомизированные алгоритмы
- Алгоритм Монте-Карло  
- Алгоритм Лас-Вегас  
- Быстрая проверка простоты чисел  

## 14. Алгоритмы для конкурентных систем
- Потокобезопасные алгоритмы  
- Алгоритмы блокировок и безблокировочные структуры  
- Алгоритмы согласованности (например, Paxos, Raft)  

## 15. Выбор алгоритма
- Критерии выбора  
- Баланс между временем и памятью  

## 16. Практические задачи
- Часто задаваемые задачи на собеседованиях  
- Оптимизация решений  

## 17. Полезные ссылки и ресурсы
- Книги  
- Онлайн-курсы  
- Платформы для практики  

---
📁 algorithms/
├── 📄 algorithms.md
├── 📁 1-introduction/
│   ├── 📄 1-introduction.md
│   ├── 📁 definition-and-classification/
│   │   └── 📄 definition-and-classification.md
│   └── 📁 complexity-analysis/
│       └── 📄 complexity-analysis.md
├── 📁 2-sorting-algorithms/
│   ├── 📄 2-sorting-algorithms.md
│   ├── 📁 basic-sorts/
│   │   └── 📄 basic-sorts.md
│   └── 📁 efficient-sorts/
│       └── 📄 efficient-sorts.md
├── 📁 3-searching-algorithms/
│   ├── 📄 3-searching-algorithms.md
│   ├── 📁 linear-search/
│   │   └── 📄 linear-search.md
│   └── 📁 binary-search/
│       └── 📄 binary-search.md
├── 📁 4-greedy-algorithms/
│   ├── 📄 4-greedy-algorithms.md
│   ├── 📁 knapsack-problem/
│   │   └── 📄 knapsack-problem.md
│   └── 📁 coin-change/
│       └── 📄 coin-change.md
├── 📁 5-dynamic-programming/
│   ├── 📄 5-dynamic-programming.md
│   ├── 📁 0-1-knapsack/
│   │   └── 📄 0-1-knapsack.md
│   └── 📁 longest-common-subsequence/
│       └── 📄 longest-common-subsequence.md
├── 📁 6-graph-algorithms/
│   ├── 📄 6-graph-algorithms.md
│   ├── 📁 dfs-and-bfs/
│   │   └── 📄 dfs-and-bfs.md
│   └── 📁 shortest-path/
│       └── 📄 shortest-path.md
├── 📁 7-divide-and-conquer/
│   ├── 📄 7-divide-and-conquer.md
│   ├── 📁 quick-sort/
│   │   └── 📄 quick-sort.md
│   └── 📁 merge-sort/
│       └── 📄 merge-sort.md
├── 📁 8-two-pointers-and-sliding-window/
│   ├── 📄 8-two-pointers-and-sliding-window.md
│   ├── 📁 substring-search/
│   │   └── 📄 substring-search.md
│   └── 📁 maximum-subarray/
│       └── 📄 maximum-subarray.md
├── 📁 9-string-algorithms/
│   ├── 📄 9-string-algorithms.md
│   ├── 📁 kmp-algorithm/
│   │   └── 📄 kmp-algorithm.md
│   └── 📁 rabin-karp/
│       └── 📄 rabin-karp.md
├── 📁 10-tree-algorithms/
│   ├── 📄 10-tree-algorithms.md
│   ├── 📁 tree-traversal/
│   │   └── 📄 tree-traversal.md
│   └── 📁 segment-tree/
│       └── 📄 segment-tree.md
├── 📁 11-recursion-and-iteration/
│   ├── 📄 11-recursion-and-iteration.md
│   ├── 📁 recursion-principles/
│   │   └── 📄 recursion-principles.md
│   └── 📁 memoization/
│       └── 📄 memoization.md
├── 📁 12-set-and-map-algorithms/
│   ├── 📄 12-set-and-map-algorithms.md
│   ├── 📁 hashing/
│   │   └── 📄 hashing.md
│   └── 📁 sweep-line/
│       └── 📄 sweep-line.md
├── 📁 13-probabilistic-algorithms/
│   ├── 📄 13-probabilistic-algorithms.md
│   ├── 📁 monte-carlo/
│   │   └── 📄 monte-carlo.md
│   └── 📁 las-vegas/
│       └── 📄 las-vegas.md
├── 📁 14-concurrency-algorithms/
│   ├── 📄 14-concurrency-algorithms.md
│   ├── 📁 thread-safe-algorithms/
│   │   └── 📄 thread-safe-algorithms.md
│   └── 📁 lock-free-structures/
│       └── 📄 lock-free-structures.md
├── 📁 15-algorithm-selection/
│   ├── 📄 15-algorithm-selection.md
│   ├── 📁 time-memory-tradeoff/
│   │   └── 📄 time-memory-tradeoff.md
│   └── 📁 practical-considerations/
│       └── 📄 practical-considerations.md
├── 📁 16-practice-problems/
│   ├── 📄 16-practice-problems.md
│   ├── 📁 interview-questions/
│   │   └── 📄 interview-questions.md
│   └── 📁 optimization/
│       └── 📄 optimization.md
└── 📁 17-resources/
    ├── 📄 17-resources.md
    ├── 📁 books/
    │   └── 📄 books.md
    └── 📁 online-courses/
        └── 📄 online-courses.md