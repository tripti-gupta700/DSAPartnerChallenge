# Day 37 — Binary Trees Interview Questions: BFS (Q1–Q9)

> **DSA Partner Challenge** | Week 6

---

## 📌 Topic of the Day

**Binary Trees FAANG Interview Questions — BFS problems (first ~2.5 hrs of the video).**

---

## 🎥 Resource

[Binary Trees Interview Questions — FAANG](https://youtu.be/9D-vP-jcc-Y?si=2Fv5ggNq4zAMNOgr) — Kunal Kushwaha

---

## 🧠 BFS Core Template

Every BFS binary tree problem uses this skeleton:

```java
Queue<TreeNode> q=new LinkedList<>(); q.offer(root);
while(!q.isEmpty()){
    int size=q.size();  // ← snapshot BEFORE the loop
    for(int i=0;i<size;i++){
        TreeNode node=q.poll();
        // process node here
        if(node.left!=null)  q.offer(node.left);
        if(node.right!=null) q.offer(node.right);
    }
    // one level done
}
```

> **Always snapshot `size=q.size()` before the for loop** — children added inside the loop would corrupt the level boundary.

---

## 💻 9 BFS Problems from the Video

---

### Q1. Binary Tree Level Order Traversal — [LC #102](https://leetcode.com/problems/binary-tree-level-order-traversal/) `Medium`

```java
Queue<TreeNode> q=new LinkedList<>(); q.offer(root);
while(!q.isEmpty()){
    int sz=q.size(); List<Integer> lvl=new ArrayList<>();
    for(int i=0;i<sz;i++){
        TreeNode n=q.poll(); lvl.add(n.val);
        if(n.left!=null) q.offer(n.left);
        if(n.right!=null) q.offer(n.right);
    }
    res.add(lvl);
}
```

---

### Q2. Average of Levels in Binary Tree — [LC #637](https://leetcode.com/problems/average-of-levels-in-binary-tree/) `Easy`

BFS template. Replace `add to list` with `sum += n.val`. After level: `res.add(sum/sz)`.

---

### Q3. Level Order Successor of a Node `Medium`

BFS. After polling a node that equals the target, return `q.peek()` — the next element in the queue is the successor.

```java
if(n.val==key) return q.peek();
```

---

### Q4. Binary Tree Zigzag Level Order Traversal — [LC #103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) `Medium`

BFS + direction flag. Use `LinkedList<Integer>` as level: `addLast` for left-to-right, `addFirst` for right-to-left. Flip flag after each level.

```java
LinkedList<Integer> lvl=new LinkedList<>();
if(leftToRight) lvl.addLast(n.val);
else            lvl.addFirst(n.val);
leftToRight=!leftToRight;
```

---

### Q5. Binary Tree Level Order Traversal II — [LC #107](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/) `Medium`

Standard BFS, but use `res.addFirst(lvl)` (LinkedList) or reverse at the end: `return res[::-1]` in Python.

---

### Q6. Populating Next Right Pointers in Each Node — [LC #116](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/) `Medium`

BFS: within the level loop, track `prev`. Set `prev.next = curr`. Last node's next stays null.

```java
Node prev=null;
for(int i=0;i<sz;i++){
    Node n=q.poll();
    if(prev!=null) prev.next=n;
    prev=n;
    if(n.left!=null) q.offer(n.left);
    if(n.right!=null) q.offer(n.right);
}
```

---

### Q7. Binary Tree Right Side View — [LC #199](https://leetcode.com/problems/binary-tree-right-side-view/) `Medium`

BFS: record the node at `i==sz-1` (last node polled in each level) — that's the rightmost visible node.

```java
if(i==sz-1) res.add(n.val);
```

---

### Q8. Cousins in Binary Tree — [LC #993](https://leetcode.com/problems/cousins-in-binary-tree/) `Easy`

Track `(depth, parent)` for both x and y during BFS. Cousins: same depth AND different parent.

```python
q=deque([(root,None,0)]); info={}
while q:
    node,parent,depth=q.popleft()
    if node.val in (x,y): info[node.val]=(depth,parent)
    if node.left:  q.append((node.left, node, depth+1))
    if node.right: q.append((node.right,node, depth+1))
return info[x][0]==info[y][0] and info[x][1]!=info[y][1]
```

---

### Q9. Symmetric Tree — [LC #101](https://leetcode.com/problems/symmetric-tree/) `Easy`

BFS with mirror pairs. Enqueue `(l.left, r.right)` and `(l.right, r.left)`.

```java
q.offer(root.left); q.offer(root.right);
while(!q.isEmpty()){
    TreeNode l=q.poll(), r=q.poll();
    if(l==null&&r==null) continue;
    if(l==null||r==null||l.val!=r.val) return false;
    q.offer(l.left); q.offer(r.right);   // outer
    q.offer(l.right); q.offer(r.left);   // inner
}
return true;
```

---

## 📊 Pattern Summary

| Problem | What changes |
|---------|-------------|
| Level Order | Collect all vals per level |
| Average of Levels | sum/count per level |
| Level Order Successor | q.peek() after finding target |
| Zigzag | addFirst vs addLast per level |
| Level Order II | addFirst to result (bottom-up) |
| Next Right Pointers | prev.next=curr within level |
| Right Side View | record i==sz-1 node |
| Cousins | track (depth,parent) per node |
| Symmetric Tree | enqueue mirror pairs |

---

## ✅ Checklist Before You Sleep

- [ ] BFS skeleton — write from memory without looking
- [ ] Snapshot `size=q.size()` before the for loop
- [ ] Level Order, Average of Levels, Zigzag, Level Order II
- [ ] Right Side View — `i==sz-1`
- [ ] Cousins — same depth, different parent
- [ ] Symmetric Tree — mirror pairs in queue
- [ ] Solved all 9 BFS problems

---

*Next up → Day 38: Binary Trees DFS Interview Questions (Q10–Q24)*
