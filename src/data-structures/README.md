# Структуры данных

## 1. Введение в структуры данных
- Определение и классификация
- Примеры применения в Backend-разработке

## 2. Базовые структуры данных
### 2.1. Массивы (Arrays)
- Одномерные и многомерные массивы  
- Динамические массивы (`ArrayList` в Java/Kotlin)  
- Временные и пространственные характеристики

### 2.2. Списки (Lists)
- Связанные списки: односвязный, двусвязный, циклический  
- Реализация (`LinkedList`)  
- Сравнение с массивами

### 2.3. Стек (Stack)
- Принцип LIFO  
- Реализация и применение (`Stack`, `Deque`)  
- Примеры задач: обратный порядок, проверка скобок

### 2.4. Очередь (Queue)
- Принцип FIFO  
- Очередь с приоритетом (`PriorityQueue`)  
- Двусторонняя очередь (`Deque`)  

## 3. Хеш-структуры
### 3.1. Хеш-таблица (Hash Table)
- Принцип работы  
- Коллизии и способы их разрешения  
- Реализация в Java/Kotlin (`HashMap`, `HashSet`)  

### 3.2. Дерево хеширования (Hash Tree, Merkle Tree)  
- Принцип работы  
- Примеры использования  

## 4. Деревья (Trees)
### 4.1. Двоичное дерево (Binary Tree)  
- Основные операции  
- Обходы дерева: in-order, pre-order, post-order  

### 4.2. Двоичное дерево поиска (Binary Search Tree, BST)  
- Вставка, удаление, поиск  
- Балансировка  

### 4.3. Самобалансирующиеся деревья  
- AVL-дерево  
- Красно-черное дерево (Red-Black Tree)  

### 4.4. Куча (Heap)  
- Мин-куча и макс-куча  
- Применение в алгоритмах (`PriorityQueue`)  

## 5. Графы (Graphs)
- Представление графов: матрица смежности, список смежности  
- Обходы: BFS, DFS  
- Взвешенные/невзвешенные графы  
- Ориентированные/неориентированные графы  
- Примеры задач  

## 6. Специализированные структуры
### 6.1. Суффиксное дерево и суффиксный массив  
### 6.2. Дерево отрезков (Segment Tree)  
### 6.3. Фенвик дерево (Binary Indexed Tree)  
### 6.4. Trie (Префиксное дерево)  

## 7. Выбор структуры данных
- Критерии выбора  
- Сравнение по времени и памяти  

## 8. Практические задачи
- Часто задаваемые задачи на собеседованиях  
- Реализация и оптимизация  

## 9. Полезные ссылки и ресурсы
- Книги  
- Онлайн-курсы  
- Платформы для практики  

---

📁 data-structures/
├── 📄 data-structures.md
├── 📁 1-introduction/
│   ├── 📄 1-introduction.md
│   └── 📁 core-concepts/
│       └── 📄 core-concepts.md
├── 📁 2-basic-structures/
│   ├── 📄 2-basic-structures.md
│   ├── 📁 arrays/
│   │   └── 📄 arrays.md
│   ├── 📁 linked-lists/
│   │   └── 📄 linked-lists.md
│   ├── 📁 stack/
│   │   └── 📄 stack.md
│   └── 📁 queue/
│       └── 📄 queue.md
├── 📁 3-hash-structures/
│   ├── 📄 3-hash-structures.md
│   ├── 📁 hash-table/
│   │   └── 📄 hash-table.md
│   └── 📁 bloom-filter/
│       └── 📄 bloom-filter.md
├── 📁 4-trees/
│   ├── 📄 4-trees.md
│   ├── 📁 binary-tree/
│   │   └── 📄 binary-tree.md
│   ├── 📁 avl-tree/
│   │   └── 📄 avl-tree.md
│   └── 📁 heap/
│       └── 📄 heap.md
├── 📁 5-graphs/
│   ├── 📄 5-graphs.md
│   ├── 📁 representation/
│   │   └── 📄 representation.md
│   └── 📁 algorithms/
│       └── 📄 algorithms.md
├── 📁 6-advanced/
│   ├── 📄 6-advanced.md
│   ├── 📁 segment-tree/
│   │   └── 📄 segment-tree.md
│   └── 📁 trie/
│       └── 📄 trie.md
└── 📁 7-practice/
    ├── 📄 7-practice.md
    ├── 📁 leetcode/
    │   └── 📄 leetcode.md
    └── 📁 real-world/
        └── 📄 real-world.md