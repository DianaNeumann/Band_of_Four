

# 1. [Обход в глубину DFS](https://neerc.ifmo.ru/wiki/index.php?title=%D0%9E%D0%B1%D1%85%D0%BE%D0%B4_%D0%B2_%D0%B3%D0%BB%D1%83%D0%B1%D0%B8%D0%BD%D1%83,_%D1%86%D0%B2%D0%B5%D1%82%D0%B0_%D0%B2%D0%B5%D1%80%D1%88%D0%B8%D0%BD)

Общая идея алгоритма состоит в следующем: для каждой не пройденной вершины необходимо найти все не пройденные смежные вершины и повторить поиск для них
```
1. Выбираем любую вершину из еще не пройденных, обозначим ее как u.
2. Запускаем процедуру dfs(u)
    • Помечаем вершину u как пройденную
    • Для каждой не пройденной смежной с u вершиной (назовем ее v) запускаем dfs(v)
3. Повторяем шаги 1 и 2, пока все вершины не окажутся пройденными
```

DFS применяется в:
- Топологической сортировке.
- Поиска точек сочленения, мостов.
- Поиска циклов


 
Время: O(n + m)

Память: O(n)


```
потом тут будет реализация

void DFS(int v) {}
```
---

# 2. [Обход в ширину BFS](https://neerc.ifmo.ru/wiki/index.php?title=%D0%9E%D0%B1%D1%85%D0%BE%D0%B4_%D0%B2_%D1%88%D0%B8%D1%80%D0%B8%D0%BD%D1%83)

```
1. Помечаем все вершины как не пройденные 
2. Добавляем в очередь стартовую вершину 
3. В цикле: (пока очередь не пустая)
    a. Берем вершину из очереди и помечаем ее как пройдённую
    b. Добавляем в очередь все смежные с ней вершины, которые не пройденные
    c. Удаляем из очереди пройденную вершину
```
BFS применяется в:
- Поиске компонент связности в графе
- Поиске кратчайшего пути между двумя узлами невзвешенного графа
- Нахождении кратчайшего цикла в ориентированном невзвешенном графе


Время: O(n + m)

Память: O(n)

---


# 3. [Топологическая сортировка](https://neerc.ifmo.ru/wiki/index.php?title=%D0%98%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%BE%D0%B1%D1%85%D0%BE%D0%B4%D0%B0_%D0%B2_%D0%B3%D0%BB%D1%83%D0%B1%D0%B8%D0%BD%D1%83_%D0%B4%D0%BB%D1%8F_%D1%82%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B9_%D1%81%D0%BE%D1%80%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%BA%D0%B8)
- Только для ациклических ориентированных графов!
- Сортирует граф, таким образом, что для любой дуги (u, v)  u  будет перед v
- По сути просто DFS, где мы каждую посещенную вершину добавляем в стэк

```
1. Выбираем любую вершину из еще не пройденных, обозначим ее как u.
2. Запускаем процедуру dfs(u)
    • Помечаем вершину u как пройденную
    • Для каждой не пройденной смежной с u вершиной (назовем ее v) запускаем dfs(v)
3. Каждую пройденную (черную) вершину помещаем в стек 
4. Повторяем шаги 1-3 пока все вершины не окажутся пройденными
```
---

# 4. Конденсация графа

Компоненты сильной связности в графе G можно найти с помощью поиска в глубину в 3 этапа:

```
    1. Построить граф H с обратными (инвертированными) рёбрами
    2. Выполнить в H поиск в глубину и найти f[u] — время окончания обработки вершины u
    3. Выполнить поиск в глубину в G, перебирая вершины во внешнем цикле в порядке убывания f[u]

```
- Полученные на 3-ем этапе деревья поиска в глубину будут являться компонентами сильной связности графа G.
- Так как компоненты сильной связности G и H графа совпадают, то первый поиск в глубину для нахождения f[u] можно выполнить на графе G, а второй — на H.

```
vector<vector<int>> graph;
vector<vector<int>> inv_graph;
bool* used;
 
vector<int> f;
int* comp;
 
void DFSfirst(int v) {
    used[v] = true;
    for (int i : graph[v]) {
        if (!used[i])
            DFSfirst(i);
    }
    f.push_back(v);
}
void DFSsecond(int v, int c_num) {
    used[v] = true;
    comp[v] = c_num;
    for (int i : inv_graph[v]) {
        if (!used[i])
            DFSsecond(i, c_num);
    } 
}
```
---

# 5. Поиск и восстановление всех видов циклов


# 6. Поиск Гамильтонова цикла

# 7. Поиск Эйлерова цикла

Для не орграфа.
```
Критерий эйлеровости (существует эйлеров цикл):
    1. Все вершины имели четную степень.
    2. Все компоненты связности кроме, может быть одной, не содержали ребер.
    
Критерий полу-эйлеровости (существует эйлеров путь):
    1. Количество вершин с нечетной степенью меньше или равно (?) двум.
    2.  Все компоненты связности кроме, может быть одной, не содержат ребер.
```
Для орграфа.
```
Критерий эйлеровости (существует эйлеров цикл):
    1. Входная степень любой вершины равна ее выходной степени.
    2. Все компоненты слабой связности кроме, может быть одной, не содержат ребер.
    
Критерий полу-эйлеровости (существует эйлеров путь):
    1. Входная степень любой вершины равна ее выходной степени, кроме двух вершин графа:
        • для одной из которых deg(+) − deg(−) = 1
        • а для другой deg(+) − deg(−) = −1
    2. Все компоненты слабой связности кроме, может быть одной, не содержат ребер.
```

Данный код протестирован для орграфа, но и с неориентированным графом должен (скорее всего) работать.
Время: O(V + E)
```
#include <iostream>
#include <stack>
#include <vector>
#include <fstream>

using std::cout;
using std::endl;

// working only with directed graph

struct Edge{
    int color = 0;
};

struct Vertex{
    std::vector<int> neighbours;
};

// todo
// it might be worth printing vertices after stack is full
void EulerDfs(std::vector<Vertex> & graph, std::vector<std::vector<Edge>> & matrix,
              std::stack<int> & stack, std::ostream & out) {
    if (stack.empty()) return;
    int v = stack.top();
    out << v << std::endl;
    stack.pop();

    for (auto u : graph[v].neighbours){
        if (matrix[v][u].color == 0){
            matrix[v][u].color = 1;
            matrix[u][v].color = 1;
            stack.push(u);
            EulerDfs(graph, matrix, stack, out);
        }
    }
}

int main() {
    std::ifstream fin("test.in");
    std::ofstream fout("res.out");

    int n, m;
    fin >> n >> m;

    std::vector<Vertex> graph (n + 1);
    std::vector<std::vector<Edge>> matrix(n + 1, std::vector<Edge> (n + 1));

    for (int i = 0; i < m; i++)
    {
        int x, y;
        fin >> x >> y;
        graph[x].neighbours.push_back(y);
    }

    std::stack<int> stack;
    stack.push(1);

    EulerDfs(graph, matrix, stack, fout);

    return 0;
}

```
---


# 8. Нахождение компонент связности в неориентированном графе

```

1. Помечаем все вершины как не пройденные 
2. Цикл пока есть не пройденные вершины
    • Запускаем обход в глубину от вершины
    • Все пройденные вершины собираем в первую компоненту 
    • Ищем не пройденную вершину
3. Выводим все компоненты графа 

```

---

# 9. [Нахождение компонент сильной связности](https://neerc.ifmo.ru/wiki/index.php?title=%D0%98%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_%D0%BE%D0%B1%D1%85%D0%BE%D0%B4%D0%B0_%D0%B2_%D0%B3%D0%BB%D1%83%D0%B1%D0%B8%D0%BD%D1%83_%D0%B4%D0%BB%D1%8F_%D0%BF%D0%BE%D0%B8%D1%81%D0%BA%D0%B0_%D0%BA%D0%BE%D0%BC%D0%BF%D0%BE%D0%BD%D0%B5%D0%BD%D1%82_%D1%81%D0%B8%D0%BB%D1%8C%D0%BD%D0%BE%D0%B9_%D1%81%D0%B2%D1%8F%D0%B7%D0%BD%D0%BE%D1%81%D1%82%D0%B8)


# 10. Алгоритмы Краскала

```

1. Сортировать ребра по неубыванию по их весам.
2. Далее с помощью вызовов функции make_set()мы каждую вершину можем поместить в свое собственное дерево, то есть, создаем некоторое множество подграфов.
3. Итерируемся по всем ребрам в отсортированном порядке и смотрим, принадлежат ли инцидентные вершины текущего ребра разным подграфам с помощью функции find_set() или нет, если оба конца лежат в разных компонентах, то объединяем два разных подграфа в один с помощью функции union_sets().

```
```
vector<int> parent, rank;

void make_set(int v) {
    parent[v] = v;
    rank[v] = 0;
}

int find_set(int v) {
    if (v == parent[v])
        return v;
    return parent[v] = find_set(parent[v]);
}

void union_sets(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b) {
        if (rank[a] < rank[b])
            swap(a, b);
        parent[b] = a;
        if (rank[a] == rank[b])
            rank[a]++;
    }
}

struct Edge {
    int u, v, weight;
    bool operator<(Edge const& other) {
        return weight < other.weight;
    }
};

int n;
vector<Edge> edges;

int cost = 0;
vector<Edge> result;
parent.resize(n);
rank.resize(n);
for (int i = 0; i < n; i++)
    make_set(i);

sort(edges.begin(), edges.end());

for (Edge e : edges) {
    if (find_set(e.u) != find_set(e.v)) {
        cost += e.weight;
        result.push_back(e);
        union_sets(e.u, e.v);
    }
}
```

Время: O(mlogm)
Память: O(n)

---


# 11. Алгоритм Прима

# 12. Алгоритм Беллмана-Форда

# 13. Алгоритм DAG - кратчайший путь в ациклическом орграфе (дпшка)

Вариант 1) Динамика вперёд:
```
1. Запускаем топсорт. Получаем вершины в топологическом порядке.
2. Заводим массив расстояний dist такой, что:
    • до start (стартовой вершины) dist(start) = 0
    • для остальных dist(i) = inf
3. Запускаеи динамику: dist(i) = min(dist(j) + w(j, i)), где j - вершина-предок для i. 

Почему нам важно смотреть вершины в порядке топ. сорта?
При проходе по графу мы всегда знаем, что для более маленькой задачи
(для вершин расположенных ранее в топсорте) результат уже вычислен.

```

Вариант 2) Динамика назад:
```
1. Заведём массив расстояний до детей.
2. Запустим BFS.
3. Для каждой вершины будем обновлять расстояние до сына в том случае, 
    если хотя бы для одного потомка в таком случае уменьшится расстояние.
```
---


# 14. Алгоритм Дейкстра с очередью/массивом



# 15. Алгоритм Флойда-Уоршалла















