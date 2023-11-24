# Bài tập Samsung - Mục A

## Bài 1

-   Đồ thị 1 không có chu trình Euler, có một chu trình Hamilton là `0 1 3 6 7 4 8 5 9 2`.
-   Đồ thị 2 không có chu trình Euler, có một chu trình Hamilton là `0 1 3 6 7 4 8 5 9 2`.
-   Đồ thị 3 không có chu trình Euler, có một chu trình Hamilton là `0 1 3 6 7 4 8 5 9 2`.
-   Đồ thị 4 không có chu trình Euler, có một chu trình Hamilton là `0 1 3 6 7 4 8 5 9 2`.

## Bài 2

Xét đồ thị đầy đủ $K_V(V_0,E_0)$ với $|V_0|=V$. Mỗi cách chọn một đồ thị vô hướng $V$ đỉnh và $E$ cạnh không có cạnh khuyên và cạnh song song ứng với một tập hợp con $E_1$ của $E_0$ có đúng $E$ phần tử, số tập hợp thoả mãn như vậy là $C_{|E_0|}^E=C_{\frac{V(V-1)}{2}}^E$

## Bài 3

_Input:_ Đồ thị $G(V,E)$ với $V = \{v_0, v_1, ..., v_n\}$

_Output:_ Tập hợp $S$ các cạnh song song thuộc đồ thị

_Mã giả:_

```
def checkMultipleEdge(G):
    n = len(G.nodes)
    mark = [] * n
    S = []
    for v in G.nodes:
        mark[v] = -1
    for u in G.nodes:
        for v in G.adj(u):
            if (mark[v] == u): S.add(v)
            mark[v] = u
    return S

```

Độ phức tạp của thuật toán được sử dụng là $O(|V| + |E|)$.

## Bài 4

_Chiều thuận:_

Xét đồ thị $G(V, E)$ không có chu trình lẻ. Tô màu $|V|$ đỉnh của đồ thị bằng hai màu xanh và đỏ theo cách sau: Tại mỗi thành phần liên thông của đồ thị, xuất phát từ một đỉnh $v_0$, ta tô màu $v_0$ màu đỏ và các đỉnh kề với $v_0$ chưa được tô màu thành màu xanh. Tiếp tục xét các đỉnh kề với các đỉnh vừa được tô màu xanh, tô màu những đỉnh chưa được tô màu thành màu đỏ, rồi lại dùng những đỉnh màu đỏ này để tô màu xanh cho các đỉnh kề chúng. Thực hiện liên tục như vậy đến khi toàn bộ các đỉnh được tô màu. Giả sử sau khi thực hiện cách tô màu trên, có hai đỉnh $v_1$ và $v_2$ kề nhau được tô cùng màu.

Gọi $d_v$ là bước mà $v$ được tô màu, coi $d_{v_0} = 0$. Dễ thấy $d_v$ là độ dài của một đường đi từ $v$ đến $v_0$. Nếu $v$ có cùng màu với $v_0$, $d_v$ là số chẵn, ngược lại $d_v$ là số lẻ. Do đó, $d_{v_1} \mod 2 = d_{v_2} \mod 2$. Vì $v_1$ và $v_2$ là hai đỉnh kề nhau nên $v_0, v_1, v_2$ cùng thuộc một chu trình lẻ; điều này mâu thuẫn với giả thiết đồ thị không có chu trình lẻ.

Vậy cách tô màu trên phủ kín $G$ mà không có hai đỉnh nào kề nhau có cùng màu, nói cách khác $G$ là đồ thị hai phía.

_Chiều nghịch:_

Giả sử trên đồ thị $K_{a_1, a_2}(V, E)$ tồn tại $2k + 1$ đỉnh $v_1$, $v_2$, ..., $v_{2k+1}$ tạo thành một chu trình theo đúng thứ tự đó $(k\in\mathbb{Z})$.
Tô màu các đỉnh thuộc chu trình trên có chỉ số chẵn màu xanh, và các đỉnh có chỉ số lẻ màu đỏ. Khi đó, tồn tại hai đỉnh có màu đỏ nằm kề nhau là $v_1$ và $v_{2k+1}$, mâu thuẫn với giả thiết đồ thị hai phía.

Do đó trên đồ thị hai phía không tồn tại chu trình lẻ.

## Bài 5

Xét đồ thị $G(V, E)$ không có đỉnh articulation (trong lời giải này tạm gọi là _khớp_) nào. Giả sử đồ thị này không có tính chất song liên thông.

Xét hai đỉnh $u, v \in V$. Do đồ thị này không song liên thông, nên mọi đường đi từ $u$ đến $v$ đều phải qua một đỉnh $w\neq u, v \in V$. Nếu đỉnh $w$ bị xoá, đồ thị sẽ mất tính liên thông, do đó $w$ là một khớp của đồ thị, mâu thuẫn với giả thiết.

Vậy một đồ thị bất kỳ không có khớp thì sẽ có tính song liên thông.

## Bài 6

Thuật toán Tarjan:

_Input:_ đồ thị $G(V, E)$ với $V = {0, 1, 2, ..., n}$

_Output:_ `true` nếu đồ thị liên thông cạnh, `false` trong trường hợp còn lại

_Mã giả:_

```
low = []
num = []
par = []
index = 0
cntBridges = 0

def dfs(u):
    index += 1
    low[u] = num[u] = index
    for v in G.adj(u):
        if (v == par[u]): continue
        if (par[v] == -1):
            low[u] = min(low[u], num[v])
        else:
            par[v] = u
            dfs(v)
            low[u] = min(low[u], low[v])
            if (low[u] == num[u]):
                cntBridges += 1


def isEdgeConnected(G):
    n = len(G.nodes)
    low = [0] * n
    num = [0] * n
    for v in G.nodes:
        if (num[v] != 0):
            par[v] = -1
            dfs(v)
    return (cntBridges != 0)
```

Độ phức tạp của thuật toán là $O(|V| + |E|)$.

## Bài 7

_Input_: Ma trận $A_{m,n}$ biểu diễn các điểm ảnh, với $A_{i, j}$ là màu của điểm ảnh $i, j$

_Output_: Danh sách các điểm ảnh kề nhau cùng màu

_Mã giả_:

```
def floodfill(a):
    sameColor = []
    m = len(a)
    n = len(a[0])
    q = Queue(int)
    q.push((0, 0))
    visited = [[False] * m] * n
    visited[0][0] = True
    while (!q.empty()):
        (ux, uy) = q.pop()
        for ((vx, vy) adjacent to (ux, uy)):
            if ((vx, vy) is out of range || visited[vx][vy]):
                continue
            if (a[vx][vy] == a[ux][uy]):
                sameColor.append(((ux, uy), (vx, vy)))
            visited[vx][vy] = True
            q.push((vx, vy))
    return sameColor
```
