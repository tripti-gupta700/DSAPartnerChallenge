# Day 26 — Difficult Backtracking

> **DSA Partner Challenge** | Week 4

---

## 📌 Topic of the Day

**Hard backtracking — N-Queens, Word Break II (Google), Unique Paths III, Landmines**

---

## 🎥 Resources

| Language | Link |
|----------|------|
| Java | [youtu.be/nC1rbW2YSz0](https://youtu.be/nC1rbW2YSz0?si=j4VjQkzlVbx3QMVV) |
| C++ #1 | [youtu.be/DOXoQfHyc7A](https://youtu.be/DOXoQfHyc7A?si=8hTFvh7trtK0lryH) |
| C++ #2 | [youtu.be/7AYJLrDxbBU](https://youtu.be/7AYJLrDxbBU?si=UA6zqZ-ITFboOI8M) |

---

## 💻 Practice Problems — 4 Hard Problems

---

### Q1. N-Queens — [LC #51](https://leetcode.com/problems/n-queens/) `Hard`

Place n queens on an n×n board so no two queens attack each other.

**Attack check:** same column, same major diagonal (row-col), same minor diagonal (row+col).

```java
// O(1) attack check with 3 boolean arrays
if (!cols[col] && !d1[row-col+n] && !d2[row+col]) { ... }
```

```python
def solveNQueens(self, n):
    result, queens = [], []
    cols, d1, d2 = set(), set(), set()
    def bt(row):
        if row==n:
            result.append(['.'*c+'Q'+'.'*(n-c-1) for c in queens]); return
        for col in range(n):
            if col not in cols and row-col not in d1 and row+col not in d2:
                queens.append(col); cols.add(col); d1.add(row-col); d2.add(row+col)
                bt(row+1)
                queens.pop(); cols.discard(col); d1.discard(row-col); d2.discard(row+col)
    bt(0); return result
```

> Major diagonal (`\`): all cells have same `row - col`. Minor diagonal (`/`): same `row + col`. Using sets/arrays makes attack check O(1) instead of O(n).

---

### Q2. Word Break II — [LC #140](https://leetcode.com/problems/word-break-ii/) `Hard` — Google

Return all sentences that segment string `s` using dictionary words.

```java
// Java with memoisation
List<String> dfs(String s, int start, Set<String> dict, Map<Integer,List<String>> memo) {
    if (memo.containsKey(start)) return memo.get(start);
    List<String> result = new ArrayList<>();
    if (start==s.length()) { result.add(""); return result; }
    for (int end=start+1; end<=s.length(); end++) {
        String word=s.substring(start,end);
        if (dict.contains(word))
            for (String rest : dfs(s,end,dict,memo))
                result.add(word+(rest.isEmpty()?"":" "+rest));
    }
    return memo.put(start,result), result;
}
```

```python
from functools import lru_cache
def wordBreak(self, s, wordDict):
    words = set(wordDict)
    @lru_cache(maxsize=None)
    def dp(start):
        if start==len(s): return ['']
        res=[]
        for end in range(start+1, len(s)+1):
            w=s[start:end]
            if w in words:
                for rest in dp(end):
                    res.append(w+(' '+rest if rest else ''))
        return res
    return dp(0)
```

> Memoisation is critical — `memo[start]` caches all sentences from that position. Without it: exponential. With it: O(n³).

---

### Q3. Unique Paths III — [LC #980](https://leetcode.com/problems/unique-paths-iii/) `Hard`

Walk from start (1) to end (2) visiting every non-obstacle (-1) cell exactly once.

```python
def uniquePathsIII(self, grid):
    m,n=len(grid),len(grid[0])
    total=sum(grid[r][c]!=-1 for r in range(m) for c in range(n))
    for r in range(m):
        for c in range(n):
            if grid[r][c]==1: sr,sc=r,c
    res=[0]
    def dfs(r,c,visited):
        if r<0 or r>=m or c<0 or c>=n or grid[r][c]==-1 or grid[r][c]==0: return
        if grid[r][c]==2:
            if visited==total: res[0]+=1; return
        tmp=grid[r][c]; grid[r][c]=0   # mark visited
        for dr,dc in [(1,0),(-1,0),(0,1),(0,-1)]:
            dfs(r+dr,c+dc,visited+1)
        grid[r][c]=tmp                  # restore (backtrack)
    dfs(sr,sc,1); return res[0]
```

> Count `total` walkable cells upfront. At end cell, check `visited == total` to confirm all visited.

---

### Q4. Find Shortest Safe Route (with Landmines) — GFG `Hard`

**Two-phase approach:**
1. Mark danger zones: for each landmine, mark it + all 8 neighbours as unsafe
2. BFS from all safe cells in column 0 → find min distance to any safe cell in last column

```python
from collections import deque
def shortest_safe_route(grid):
    R, C = len(grid), len(grid[0])
    safe = [[1]*C for _ in range(R)]
    # Phase 1: mark unsafe (8 directions around each mine)
    for r in range(R):
        for c in range(C):
            if grid[r][c]==0:
                safe[r][c]=0
                for dr,dc in [(-1,0),(1,0),(0,-1),(0,1),(-1,-1),(-1,1),(1,-1),(1,1)]:
                    nr,nc=r+dr,c+dc
                    if 0<=nr<R and 0<=nc<C: safe[nr][nc]=0
    # Phase 2: BFS (4 directions for movement)
    dist=[[-1]*C for _ in range(R)]
    q=deque()
    for r in range(R):
        if safe[r][0]: dist[r][0]=0; q.append((r,0))
    while q:
        r,c=q.popleft()
        for dr,dc in [(-1,0),(1,0),(0,-1),(0,1)]:
            nr,nc=r+dr,c+dc
            if 0<=nr<R and 0<=nc<C and safe[nr][nc] and dist[nr][nc]==-1:
                dist[nr][nc]=dist[r][c]+1; q.append((nr,nc))
    last=[dist[r][C-1] for r in range(R) if safe[r][C-1] and dist[r][C-1]!=-1]
    return min(last) if last else -1
```

---

## ✅ Checklist Before You Sleep

- [ ] N-Queens: I know the diagonal trick — row-col and row+col arrays for O(1) check
- [ ] Word Break II: memoisation at each start position — `dp[start]` = all sentences from here
- [ ] Unique Paths III: count total walkable cells, check at end cell
- [ ] Landmines: phase 1 = 8-dir danger zone marking, phase 2 = 4-dir BFS
- [ ] I solved all 4 problems
- [ ] I understand why all 4 are Hard

---

## 💬 Community

N-Queens is the most iconic backtracking problem in DSA. Drop your trace for n=4 in the community.

**Solved all 4? Drop a 🔥 in the community.**

---

*Next up → Day 27: Hashing — HashMap, HashSet, frequency maps*
