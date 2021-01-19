#### DFS/BFS 알고리즘 문제풀이 002

**미로 탈출**

동빈이는 N x M 크기의 직사각형 형태의 미로에 갇혀 있다. 미로에는 여러 마리의 괴물이 있어 이를 피해 탈출해야 한다. 동빈이의 위치는 (1,1)이고 미로의 출구는 (N,M)의 위치에 존재하며 한번에 한 칸씩 이동할 수 있다. 이때 괴물이 있는 부분은0, 없는 부분은 1로 표시된다.
미로는 반드시 탈출할 수 있는 형태로 제시된다.
이때, 동빈이가 탈출하기 위해 움직여야 하는 최소 칸의 개수를 구하시오.
칸을 셀 때는 시작 칸과 마지막 칸을 모두 포함해서 계산한다.

입력 조건
- 첫째 줄에 정수 n,m(4<= n,m <=200)이 주어진다.
- 다음 n개의 줄에는 각각 m개의 정수로 미로의 정보가 주어진다.
- 각각의 수들은 공백 없이 붙어서 입력으로 제시된다.
- 시작칸과 마지막칸은 항상 1이다

출력 조건
- 첫째 줄에 최소 이동 칸의 개수를 출력한다.

입력 예시
```
5 6
101010
111111
000001
111111
111111
```
출력 예시
```
10
```

**나만의 문제 해설**

이 문제는 BFS 관련한 문제이다.
따라서 deque를 사용하기 위해 다음과 같은 선언을 해야한다
```
from collctions import deque
```

이후 DFS 문제와 같이 그래프 초기화를 해준다.

```
graph = []
for i in range(n):
    graph.append(list(map(int,input())))
```
이 초기화 방법은 앞으로도 자주 쓰일거 같으니 외워두자

이제 이 문제는 앞서 풀어보았던 DFS문제와 다르게 이동할 네 방향을 정의해줘야한다.
```
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]
```
이게 상,하,좌,우를 정의한것인데, 아직까지 이것을 잘 활용하는 방법을 모르겠다...
내가 이해하는 방법이 조금 다른것 같다. 계속 숙지할 수 있도록 많이 써보는수밖에 없을것 같다.

이후 BFS 소스 코드를 구현하자.(사실 이부분을 못해서 이 알고리즘은 못푸는거다.)

```
def bfs(x, y):
    queue = deque()
    queue.append((x, y))

    while queue:
        x, y = queue.popleft()

        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]

            if nx < 0 or ny < 0 or nx >= n or ny >= m:
                continue
            fi graph[nx][ny] == 0:
                continue

            if graph[nx][ny] == 1:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
    return graph[n-1][m-1]
```
이게 BFS 소스코드이다. 하나하나 분석하면서 온전히 내걸로 만들 수 있도록 해보자.
```
queue = deque()
queue.append((x, y))
```
큐를 deque()를 이용하여 선언하고 함수에 들어온 x,y좌표를 queue에 삽입하는 구조이다.
큐 구현을 위해 deque 라이브러리를 이용하였다.
```
while queue:
    x, y = queue.popleft()
```
큐가 빌 때까지 반복한다는 뜻이다. 사실 이 부분은 내 머리로 코드를 작성하지 못한다.
아직 파이썬이 익숙하지 않아서 그런 것 같다.
그리고 x, y에 큐에 맨 처음 들어온 값을 집어넣는다.
참고로 queue.popleft()를 실행하면 queue의 맨 왼쪽에 있는 값이 return된다.
```
for i in range(4):
    nx = x + dx[i]
    ny = y + dy[i]
```
현재 위치에서 4방향으로의 위치를 확인한다.
dx와 dy로 정의한 부분을 사용하는 곳이다. 위치 이동할땐 다음과 같이 이동하는 형식을 사용하므로 머리속에 집어넣도록 한다.
하나하나 따져보자면 -1,0 으로 이동, 1,0으로 이동, 0,-1로 이동, 0,1로 이동이다.
```
if nx < 0 or ny < 0 or nx >= n or ny >= m:
    continue
if graph[nx][ny] == 0:
    continue
```
이 부분은 미로 찾기 공간을 벗어나는 경우와 0이라는 숫자를 마주쳤을때 무시하도록 작성된 코드이다.
```
if graph[nx][ny] == 1:
    graph[nx][ny] = graph[x][y] + 1
    queue.append((nx, ny))
```
이 소스코드는 해당 노드를 처음 방문하는 경우에는 최단 거리 기록을 하는 소스코드이다.
예를들어 0,0에서 1,0으로 방문했는데 처음 방문했으므로 그 위치 노드는 2라는 값이 된다.

이 문제는 노드를 지나가면서 새롭게 방문하는 노드는 값을 증가시키는 형태로 문제를 풀이한다.

아직 제대로 이해하지 못한 것 같지만 문제를 여러개 풀다보면 이해가 갈거라 생각한다.
마지막으로 소스코드를 정리하고 마무리를 한다.






---

```
from collections import deque

n, m = map(int, input().split())

graph = []
for i in range(n):
    graph.append(list(map(int, input())))
    
# 이동할 네 가지 방향 정의 ( 상, 하, 좌, 우)
dx = [-1, 1, 0, 0]
dy = [0, 0, -1, 1]

def bfs(x, y):
    
    # 큐 구현을 위해 deque 라이브러리 사용
    queue = deque()
    queue.append((x, y))
    # 큐가 빌 때까지 반복하기
    while queue:
        x, y = queue.popleft()
        # 현재 위치에서 4가지 방향으로의 위치 확인
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            # 미로 찾기 공간을 벗어난 경우 무시
            if nx < 0 or nx >= n or ny < 0 or ny >= m:
                continue
            # 벽인 경우 무시
            if graph[nx][ny] == 0:
                continue
            # 해당 노드를 처음 방문하는 경우에만 최단 거리 기록
            if graph[nx][ny] == 1:
                graph[nx][ny] = graph[x][y] + 1
                queue.append((nx, ny))
    # 가장 오른쪽 아래까지의 최단 거리 반환
    return (graph[n - 1][m - 1])

print(bfs(0,0))
```