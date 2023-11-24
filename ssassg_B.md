# Bài tập Samsung - Mục B

## Bài 8

Thuật toán sẽ không cho thứ tự topo trong trường hợp một đỉnh có khoảng cách ngắn hơn một đỉnh khác đến nguồn nhưng lại có bậc vào lớn hơn. Một ví dụ là đồ thị $G(V, E), V=\{1,2,3\}, E=\{(1,3),(3,2),(1,2)\}$, có thứ tự theo cách đánh BFS là $1\rightarrow 2\rightarrow 3$, còn thứ tự topo là $1\rightarrow 3\rightarrow 2$.

## Bài 9

Thuật toán tìm chu trình Euler trên đồ thị có hướng:

_Input:_ Đồ thị có hướng $G(V,E)$

_Output:_ Chu trình Euler trên đồ thị

_Mã giả:_

```


def search(u, eulerPath):
    for e in <các cạnh kề u>:
        v = <đầu còn lại>
        eulerPath.append(e)
        <xoá e khỏi danh sách cạnh kề u>
        search(v, eulerPath)

def eulerCircuit(G):
    if (<G không liên thông yếu>):
        return
    for u in G.nodes:
        if (<bậc ra u> != <bậc vào u>):
            return
    eulerPath = []
    for u in G.nodes:
        search(u, eulerPath)
    return eulerPath
            
```

_Độ phức tạp dự kiến:_ $O(N + M)$

_Chứng minh_:

Nhận xét: Đồ thị có hướng $G$ có chu trình Euler khi và chỉ khi nó liên thông yếu và mọi đỉnh của nó đều có bậc ra bằng bậc vào.

Chứng minh chiều thuận: Trên đồ thị có chu trình Euler, hiển nhiên đồ thị phải liên thông mạnh vì mọi đỉnh của $G$ phải đi đến được mọi đỉnh khác, tức là đồ thị cũng liên thông yếu. Để tạo thành chu trình, số lần đi ra và đi vào một đỉnh phải bằng nhau, đồng nghĩa với bậc ra và bậc vào của nó bằng nhau.

Chứng minh chiều đảo: Trên đồ thị liên thông yếu, xuất phát từ một đỉnh $v_0$ ta thử duyệt toàn bộ đồ thị, với điều kiện không có cạnh nào được đi qua hai lần. Mỗi lần thăm một đỉnh, ta đều sử dụng một cạnh vào và một cạnh ra ở đỉnh đó, trừ đỉnh đầu tiên. Vì mọi đỉnh đều có bậc ra bằng bậc vào, nên luôn tồn tại một cách đi từ $v_0$ đến chính nó mà không đi qua cạnh nào hai lần. Ta tiếp tục chọn một đỉnh $v_1$ nằm trên $v_0$ còn cạnh liên thuộc nó chưa bị đi qua (luôn chọn được do đồ thị đảm bảo liên thông yếu) và thực hiện lại quá trình trên; ta cũng sẽ thu được một chu trình. Do đồ thị liên thông yếu nên luôn tồn tại một đường đi từ $v_0$ đến $v_1$ hoặc ngược lại, nên chu trình tạo ra từ $v_1$ có thể nối tiếp vào chu trình tạo ra từ $v_0$. Thực hiện quá trình trên tới khi không còn cạnh nào chưa được xét, ta thu được chu trình Euler.

Thuật toán trên sử dụng chứng minh của chiều đảo nhận xét trên, và do vậy đúng.

## Bài 10

**Thuật toán tìm thành phần liên thông mạnh (Kosaraju-Sharir)**:

_Input_: Đồ thị $G(V, E)$

_Output_: Danh sách các thành phần liên thông mạnh trên $G$

_Mã giả_:

```
mk = []
path = []

def rev(g):
    <trả về đồ thị gồm các cạnh thuộc G được định chiều ngược lại>

def dfs(u, g):
    for v in g.adj(u):
        if (mk[v]) continue
        mk[v] = True
        dfs(v, g)
        path.append(v)

def stronglyConnected(g):
    n = len(g.nodes)
    mk = [False] * n
    reversePostOrder = []
    for u in range(0, n):
        if (not mk[u]):
            mk[u] = 1
            path = []
            dfs(u, rev(g))
            reversePostOrder += path
    mk = [False] * n
    res = []
    for u in reversePostOrder:
        if (not mk[u]):
            mk[u] = 1
            path = []
            dfs(u, g)
            res.append(path)
    return res
```

## Bài 11:

_Input:_ Đồ thị có hướng $G(V, E)$ không có chu trình

_Output:_ Đường đi Hamilton trên đồ thị nếu có

_Mã giả:_

```
deg = []

def topoSort(g):
    res = []
    n = len(g.nodes)
    deg = [0] * n
    for u in g.nodes:
        for v in adj(u):
            deg[v] += 1
    Queue q
    for u in g.nodes:
        if (deg[u] == 0):
            res.push(u)
            q.push(u)
    while (not q.empty()):
        u = q.pop()
        for v in adj(u):
            deg[v] -= 1
            if (deg[v] == 0):
                res.push(v)
                q.push(v)
    return res


def hamiltonDAG(g):
    path = topoSort(g)
    for i in range(1, len(path)):
        u = path[i - 1]
        v = path[i]
        if (not (u, v) in g.edges):
            return False
    return True
```

## Bài 12
Như bài 11

## Bài 13
Xét tập hợp $V$ gồm các đỉnh. Số lượng đồ thị có hướng không có cạnh song song tạo thành từ các đỉnh trên bằng số tập con của tập hợp $V \times V$ (tích Decartes) và bằng $2^{V^2}$.

## Bài 15
_Input:_ Đồ thị có hướng $G(V, E)$ không có chu trình.

_Output:_ Thứ tự topo của đồ thị

_Mã giả:_
```
deg = []

def topoSort(g):
    n = len(g.nodes)
    deg = [0] * n
    for u in g.nodes:
        for v in adj(u):
            deg[v] += 1 
    res = []
    Queue q
    for u in g.nodes:
        if (deg[u] == 0):
            res.push(u)
            q.push(u)
    while (not q.empty()):
        u = q.pop()
        for v in adj(u):
            deg[v] -= 1
            if (deg[v] == 0):
                res.push(v)
                q.push(v)
    return res
```
