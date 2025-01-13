计概机考cheatsheet（期末特供）

一.DFS和BFS的常见模板：

1.一个矩阵的搜索：

DFS（直接写）栈

```python
directions=[( )]
def dfs(i,j,grid,m,n,visited):
    stack=[(i,j)]
    visited[i][j]=True
    while stack:
        x,y=stack.pop()
        if :#中止条件
            return
        for dx,dy in directions:
            nx,ny=x+dx,y+dy
            if 0<=nx<m and 0<=ny<n and not visited[nx][ny]:
                if grid[nx][ny]  :     #判断条件
                    stack.append((nx,ny))
                    visited[nx][ny]=True
```

DFS（递归实现）

```python
directions=[()]
def dfs(i,j,grid,visited,m,n,count):
    if    :   #中止条件
        return count
    if not visited[i][j]:
    	visited[i][j]=true
        for dx,dy in directions:
            nx,ny=i+dx,j+dy
            if 0<=nx<m and 0<=ny<n and not visited[nx][ny]:
                if grid[nx][ny]   :    #判断条件
    				dfs(nx,ny,grid,visited,m,n,count+1)
```

BFS（直接写）队列

```python
from collections import deque
directions=[()]
def bfs(i,j,grid,m,n,visited):
    queue=[(i,j)]
    visited[i][j]=True
    while queue:
        for _ in range(len(queue)):
	        x,y=queue.popleft()
            if :#中止条件
                return
    	    for dx,dy in direcitons:
                nx,ny=x+dx,y+dy
                if 0<=nx<m and 0<=ny<n and not visited[nx][ny]:
                    if grid[nx][ny]     :#判断条件
                        queue.append((nx,ny))
                        visited[nx][ny]=True
                     
```

2.比较常见的图搜索：

通常图搜索是会给定一系列节点，然后给出节点中的路径。这之后和上面很像。重点：使用字典来记录路径！

```python
graph={i:[] for i in range(n)} #或者是graph=defaultdict(list)
for i in range(m):
    a,b=map(int,input().split())
    graph[a].append(b)
    graph[b].append(a)
#这就构造好了一个图
visited=set() #构造访问结果
for i in range(n):
    stack=[i]
    if i not in visited:
        visited.add(i)
        while stack:
            x=stack.pop()
            for y in graph[x]:
                if y not in visited:
                    satck.append(y)
                    visited.add(y)     #这是DFS，BFS同理。这样就可以将所有的全部搜索一遍。
```

3.带有权重的搜索

迪杰斯特拉算法：

```python
import heapq
def dijkstra(graph,start):
    n=len(graph)
    distances={i:float('inf') for i in range(n)}
    distances[start]=0
    heap=[(0,start)]
    while heap:
        current_distance,now=heapq.heappop(heap)
        if current_distance>distances[now]:
            continue
        for neighbor,weight in graph[now]:
            distance=current_distance+weight
            if distance<distances[neighbor]:
                distances[neighbor]=distance
                heapq.heappush(heap,(distance,neighbor))
     return distances

```

这是常见的dijkstra算法，下面来个具体的：

```python
import heapq
directions=[(0,1),(0,-1),(1,0),(-1,0)]
def dfs(x1,y1,x2,y2,matrix,m,n):
	if matrix[x1][y1]=='#' or matrix[x2][y2]=='#':
 		return 'NO'
 	distance=[[float('inf')]*n for _ in range(m)]
 	distance[x1][y1]=0
 	pq=[(0,x1,y1)]
 	while pq:
 		cost,x,y=heapq.heappop(pq)
 		if (x,y)==(x2,y2):
 			return cost
		for dx,dy in directions:
 			nx,ny=x+dx,y+dy
 			if 0<=nx<m and 0<=ny<n and matrix[nx][ny]!='#':
	 			new_cost=cost+abs(int(matrix[nx][ny])-int(matrix[x][y]))    
#直接dfs过不了，会超时，要使用Dijkstra算法。这个算法有贪心的思想，核心是使用堆来极快提升查找速度
				if new_cost<distance[nx][ny]:
 					distance[nx][ny]=new_cost
					heapq.heappush(pq,(new_cost,nx,ny))  
                #可以发现这个算法的代码看起来和dfs没有差太多，最主要是通过堆来优化。
	return 'NO'
 
m,n,p=map(int,input().split())
a=[input().split() for _ in range(m)]
case=[list(map(int,input().split())) for _ in range(p)]
answer=[]
for x1,y1,x2,y2 in case:
result=dfs(x1,y1,x2,y2,a,m,n)
answer.append(result)
for s in answer:
    print(s)
```

这是走山路，与常规BFS的唯一区别是采用了堆来优化路径。

二.dp（动态规划）

动态规划的基本模板有以下几个：

1.背包问题：

零一背包：n个物品只能0或1，有一个总体的限制m

```python
n,m=map(int,input().split())
w=list(map(int,input().split()))
c=list(map(int,input().split()))
dp=[[0]*(m+1) for _ in range(n+1)]
for i in range(1,n+1):
    for j in range(m+1):
        if w[i-1]<=j:
            dp[i][j]=max(dp[i-1][j],dp[i-1][j-w[i-1]]+c[i-1])
        else:
            dp[i][j]=dp[i-1][j]
print(dp[n][m])
```

完全背包

```python
n,m=map(int,input().split())
w=list(map(int,input().split()))
c=list(map(int,input().split()))
dp=[0]*(m+1)
for i in range(n):
    for j in range(w[i],m+1):
        dp[j]=max(dp[j],dp[j-w[i]]+c[i])
print(dp[m])
```

2.最长递增子序列（递减也可以）

```python
def dp(a):
    n=len(a)
    dp=[1]*(n+1)
    dp[0]=0
    for i in range(1,n+1):
        for j in range(i):
            if a[i-1]>a[j]:
                dp[i]=max(dp[j]+1,dp[i])
    return max(dp)
```

这个也可以直接二分查找：

```python
import bisect
def dp(a):
    n=len(a)
    sub=[]
    for i in range(n):
        pos=bisect.bisect_left(sub,a[i])
        if pos==len(sub):
            sub.append(a[i])
        else:
            sub[pos]=a[i]
  	return len(sub)
```

3.公共子序列

```python
def dp(words1,words2):
	n=len(words1)
    m=len(words2)
    dp=[[0]*(m+1) for _ in range(n+1)]
    for i in range(1,n+1):
        for j in range(1,m+1):
            if words1[i]==words2[j]:
	            dp[i][j]=max(dp[i][j],dp[i-1][j-1]+1)
            else:
                dp[i][j]=max(dp[i-1][j],dp[i][j-1])
    return dp[n][m]
```

4、股票问题：

这类问题通常会需要两个dp数组，也可能需要三个或更多。

土豪问题

```python
a=list(map(int,input().split(',')))
n=len(a)
dp=[0]*n
dp[0]=a[0]
max_value=a[0]
for i in range(1,n):
    dp[i]=max(dp[i-1]+a[i],a[i])
    max_value=max(max_value,dp[i])
dp_skip=[0]*n
max_value_skip=a[0]
for i in range(1,n):
    dp_skip[i]=max(dp_skip[i-1]+a[i],dp[i-1])  #跳过当前和不跳过当前。
    max_value_skip=max(dp_skip[i],max_value_skip)
max_value=max(max_value_skip,max_value)
print(max_value)
```

5、双dp：这个非常重要，当解决一个dp问题不好用时就直接考虑双dp。这里非常典型的有股票购买，还有购物时可以跳过一个，再比如说红蓝玫瑰。标志：有可能存在两个状态时，就要考虑双dp。

红蓝玫瑰

```python
s=input()
n=len(s)
dp_1=[0]*n
dp_2=[0]*n
if s[0]=='R':
    dp_2[0]=1
if s[0]=='B':
    dp_1[0]=1
for i in range(1,n):
    if s[i]=='R':
        dp_1[i]=min(dp_2[i-1]+1,dp_1[i-1])
        dp_2[i]=min(dp_2[i-1]+1,dp_1[i]+1)
    if s[i]=='B':
        dp_2[i]=min(dp_1[i-1]+1,dp_2[i-1])
        dp_1[i]=min(dp_2[i]+1,dp_1[i-1]+1)
print(dp_1[n-1])
```

卡丹算法（Kadane）

这是通过dp来寻找最大子数组和：

```python
def kadane(arr):
    # 初始化
    current_sum = arr[0]
    max_sum = arr[0]
    
    for num in arr[1:]:
        # 更新 current_sum，决定是否加入当前元素
        current_sum = max(num, current_sum + num)
        
        # 更新 max_sum
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

对于二维数组寻找最大矩阵，枚举 l~r 列的子式，按行求和后，求这些和的最大子序列即可。

```python
def kadane(arr):
    # 使用Kadane算法求解一维最大子数组和
    current_sum = arr[0]
    max_sum = arr[0]
    for num in arr[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    return max_sum

def maxSumSubmatrix(matrix):
    if not matrix or not matrix[0]:
        return 0
    
    rows, cols = len(matrix), len(matrix[0])
    max_sum = float('-inf')
    
    # 遍历每一对行 i 和 j
    for i in range(rows):
        # 初始化列和数组 sum
        sum_arr = [0] * cols
        for j in range(i, rows):
            # 对每一列进行累加
            for k in range(cols):
                sum_arr[k] += matrix[j][k]
            
            # 在 sum_arr 数组上应用 Kadane 算法
            current_max = kadane(sum_arr)
            
            # 更新全局最大子矩阵和
            max_sum = max(max_sum, current_max)
    
    return max_sum
```

三、一些其他的小算法：

1、下一个全排列：

```python
def next_permutation(nums):
    i = len(nums) - 2
    while i >= 0 and nums[i] >= nums[i + 1]:
        i -= 1
    if i >= 0:
        j = len(nums) - 1
        while nums[j] <= nums[i]:
            j -= 1
        nums[i], nums[j] = nums[j], nums[i]
    nums[i + 1:] = reversed(nums[i + 1:])
    return nums
```

2、排队，交换两人，使得身高的字典序最小：

```python
from collections import deque
N,D=map(int,input().split())
F=deque(int(input()) for _ in range(N))
M=[]

while F:
    inlist=[]
    max=F[0]
    min=F[0]
    for _ in range(len(F)):
        h=F.popleft()
        if abs(h-max)<=D and abs(h-min)<=D:
            inlist.append(h)
        else:
            F.append(h)    #不会改变原来列表的顺序。
        if h>max:
            max=h
        if h<min:
            min=h
    M+=sorted(inlist)
for m in M:
    print(m)

```

核心：将列表分割开来

3、常见内置函数：

1、heap：堆，最近用的很多，主要是方便找出最大最小值：

```python
import heapq
heapq.heappush(a,x)
heapq.heappop(a)
```

2、二分查找bisect要求必须是有序的

```python
import bisect
pos=bisect.bisect_left(a,x) #从左往右找第一个数出现的位置，或者找这个数应该插入的位置
pos=bisect.bisect_right(a,x)  #同上
```

3、队列deque

没有什么好说的，过

4、queue

```python
put(item)#放入队列，如果满了就放不进namedtup
get()#取出
empty()#判断是否为空
full()#判断是否满了
qsize()#判断是否满了
```

5、一些数据结构 collections

```python
import collections
a1=collections.defaultdict(int)#一个空字典，字典的值是整数
a2=collections.Ordereddict()#有序字典
a3=collections.namedtuple()#命名，没啥用
```

6、遍历product生成所有排列组合

```python
import itertools
for item in itertools.product('AB',repeat=2):
    print('item')
itertools.product(s1,s2,s3,s4)#拼凑
for item in itertools.permutations(dict,r)#这是把一个列表中的元素作为枚举对象，给出其排列组合
	enumerate(item)#返回一个元素和它的索引
```

7、拷贝copy

```python
import copy
a=[1,2,3,4]
b=copy.deepcopy(a)#返回来的b是一个和a一样但是完全独立的列表
```

8、ASCII码的转换

```python
ord('A')#字符转换为码
chr(65)#码转换为字符
```

9、筛法，欧拉筛和埃氏筛

埃氏筛：

```python
def sieve_of_eratosthenes(n):
    # 初始化布尔数组，默认所有数都是素数
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False  # 0 和 1 不是素数
    
    # 从 2 开始，标记合数
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    
    # 返回所有素数
    primes = [i for i in range(2, n + 1) if is_prime[i]]
    return primes

# 示例：找出小于等于 30 的所有素数
print(sieve_of_eratosthenes(30))

```

欧拉筛：

```python
def euler_sieve(n):
    # 初始化布尔数组，默认所有数都是素数
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False  # 0 和 1 不是素数
    primes = []
    
    # 遍历所有数字，筛选出素数
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
        # 对于每个素数，标记它的倍数为合数
        for prime in primes:
            if i * prime > n:  # 如果乘积超出范围，跳出
                break
            is_prime[i * prime] = False
            if i % prime == 0:  # 防止重复标记
                break
    
    return primes

# 示例：找出小于等于 30 的所有素数
print(euler_sieve(30))

```

9、进制转换

直接二进制：

```python
erjinzhi=bin(a)#把a转换为二进制，一般会有前缀0b，所以要切片
erjinzhi_1=bin(a)[2:]
b=int(erjinzhi_1,2)#这里前面的数不能有前缀0b。后面则是需要转换的进制数
```

八进制和十六进制则分别oct()和hex()但是注意还是要切片

10、回溯，这个我一直不会

八皇后问题

```python
list1 = []
	def queen(s):
 		if len(s) == 8:
 			list1.append(s)
 			return
 		for i in range(1, 9):
 			if all(str(i) != s[j] and abs(len(s) - j) != abs(i - int(s[j])) for j in range(len(s))):
				queen(s + str(i))
queen('')
samples = int(input())
for k in range(samples):
	print(list1[int(input()) - 1])
```

一些别的小东西：

读取多行输入输出：

```python
#使用循环
while True:
    try:
        line=input()
        print(line)
    except EOFerror:
        break
#或者直接sys
import sys
data=sys.stdin.read()
print(data)
```

brute force的要点：

brute force 我一直不是很会，基础思路都有，但就是语法写不对，导致写不出来

几个常见的：

1、product 两个对象的笛卡尔积

```python
product([1,2],[3,4])#对于两个列表迭代
product([1,2],repeat=2)#两个[1,2]来迭代
```

2、permutations生成排列

```python
permutations(a,r)#a是一个列表，从a中取r个元素来排列
```

日期时间：

```python
import calendar,datetime
print(calendar.isleap(2020))
print(datetime.datetime(2023,10,30).weekday())#星期几
```

