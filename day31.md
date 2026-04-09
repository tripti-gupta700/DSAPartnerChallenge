# Day 31 — Linked List: From Scratch

> **DSA Partner Challenge** | Week 5

---

## 📌 Topic of the Day

**Linked List — build it from scratch in Java, C++, and Python.**

---

## 🎥 Resources

| Language | Links |
|----------|-------|
| Java | [youtu.be/58YbpRDc4yw](https://youtu.be/58YbpRDc4yw?si=Vp_TXc55zZrUATS_) |
| C++ | [youtu.be/Nq7ok-OyEpg](https://youtu.be/Nq7ok-OyEpg?si=MtzP5vB2MPW0aQvp) · [youtu.be/VaECK03Dz-g](https://youtu.be/VaECK03Dz-g?si=VOgZu5qxaGbIxQ8w) · [youtu.be/0eKMU10uEDI](https://youtu.be/0eKMU10uEDI?si=1MF8EIem0YX8M8F_) · [youtu.be/u3WUW2qe6ww](https://youtu.be/u3WUW2qe6ww?si=mFFfabcrGXRRUnYo) |

---

## 🧠 Part 1 — What is a Linked List?

A Linked List is a linear data structure where nodes are stored at **non-contiguous** memory locations. Each node holds data and a pointer to the next node.

```
[ 10 | • ] → [ 20 | • ] → [ 30 | • ] → [ 40 | null ]
  head                                     tail
```

| | Array | Linked List |
|--|-------|-------------|
| Memory | Contiguous | Non-contiguous |
| Access by index | O(1) | O(n) |
| Insert at head | O(n) | **O(1)** |
| Insert at tail | O(1)* | O(n) |
| Delete at head | O(n) | **O(1)** |
| Search | O(n) | O(n) |

---

## 🧠 Part 2 — The Node

```java
class Node {
    int data;
    Node next;
    Node(int data) { this.data = data; this.next = null; }
}
```
```python
class Node:
    def __init__(self, data): self.data = data; self.next = None
```
```cpp
struct Node {
    int data; Node* next;
    Node(int d) : data(d), next(nullptr) {}
};
```

---

## 🧠 Part 3 — Singly Linked List Operations

```java
// Insert at head — O(1)
void insertAtHead(int data) {
    Node newNode = new Node(data);
    newNode.next = head;
    head = newNode;
}

// Insert at tail — O(n)
void insertAtTail(int data) {
    Node newNode = new Node(data);
    if (head==null) { head=newNode; return; }
    Node curr=head;
    while(curr.next!=null) curr=curr.next;
    curr.next = newNode;
}

// Delete at head — O(1)
void deleteAtHead() { if (head!=null) head = head.next; }

// Delete at tail — O(n)
void deleteAtTail() {
    if (head==null) return;
    if (head.next==null) { head=null; return; }
    Node curr=head;
    while(curr.next.next!=null) curr=curr.next;  // stop at 2nd last
    curr.next = null;
}

// Print — O(n)
void print() {
    Node curr=head;
    while(curr!=null){ System.out.print(curr.data+" → "); curr=curr.next; }
    System.out.println("null");
}
```

---

## 🧠 Part 4 — Doubly Linked List

Each node has **prev + next** pointers — bidirectional traversal.

```
null ← [prev|10|next] ⇄ [prev|20|next] ⇄ [prev|30|next] → null
```

```java
class DNode { int data; DNode prev, next; DNode(int d){data=d;} }

// Insert at head
void insertAtHead(int data) {
    DNode n = new DNode(data);
    n.next = head;
    if (head!=null) head.prev = n;
    head = n;
}

// Delete (given node reference) — O(1)
void deleteNode(DNode node) {
    if (node.prev!=null) node.prev.next = node.next;
    else head = node.next;
    if (node.next!=null) node.next.prev = node.prev;
}
```

---

## 🧠 Part 5 — Circular Linked List

Last node's `next` points back to `head`.

```
[ 10 ] → [ 20 ] → [ 30 ] → [ 40 ] ─┐
  ↑___________________________________|
```

```java
// Traverse
Node curr = head;
do {
    System.out.print(curr.data + " → ");
    curr = curr.next;
} while (curr != head);   // stop when back at head
```

---

## 💻 Practice Problems — 5 Beginner Problems

---

### Q1. Print Linked List (Traverse)
```java
Node curr = head;
while (curr != null) { System.out.print(curr.data+" "); curr=curr.next; }
```

---

### Q2. Count Nodes — GFG `Easy`
```java
int countNodes(Node head) {
    int count=0; Node curr=head;
    while(curr!=null){ count++; curr=curr.next; }
    return count;
}
```
```python
def count_nodes(head):
    cnt, curr = 0, head
    while curr: cnt+=1; curr=curr.next
    return cnt
```

---

### Q3. Search a Key in Linked List — GFG `Easy`
```java
boolean search(Node head, int key) {
    Node curr=head;
    while(curr!=null){ if(curr.data==key) return true; curr=curr.next; }
    return false;
}
```

---

### Q4. Middle of the Linked List — [LC #876](https://leetcode.com/problems/middle-of-the-linked-list/) `Easy`

**Fast & Slow pointer — THE most important LL technique:**
```java
ListNode middleNode(ListNode head) {
    ListNode slow=head, fast=head;
    while(fast!=null && fast.next!=null){
        slow=slow.next;         // move 1 step
        fast=fast.next.next;    // move 2 steps
    }
    return slow;   // slow is at middle
}
```
```python
def middleNode(self, head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next; fast = fast.next.next
    return slow
```

> When `fast` reaches end (n steps), `slow` is at n/2 = middle. ✓

---

### Q5. Delete Last Node of Linked List — GFG `Easy`
```java
Node deleteLastNode(Node head) {
    if(head==null || head.next==null) return null;
    Node curr=head;
    while(curr.next.next!=null) curr=curr.next;  // stop at 2nd last
    curr.next=null;
    return head;
}
```
```python
def delete_last_node(head):
    if not head or not head.next: return None
    curr = head
    while curr.next.next: curr = curr.next
    curr.next = None
    return head
```

---

## ✅ Checklist Before You Sleep

- [ ] I understand Node — data + next pointer
- [ ] I can implement SLL from scratch: insertAtHead/Tail, deleteAtHead/Tail, print, length
- [ ] I know SLL vs DLL vs Circular — when to use each
- [ ] I solved all 5 beginner problems
- [ ] I know the fast & slow pointer trick for finding the middle
- [ ] I know the "stop at 2nd-to-last" pattern for deleting the last node

---

## 💬 Community

Linked lists are the gateway to Trees, Graphs, and Dynamic Memory Management. If you've understood the Node structure today — you've understood the most important thing.

**Solved all 5? Drop a 🔥 in the community.**

---

*Next up → Day 32: Linked List — Medium problems (Reverse LL, Detect Cycle, Merge Sorted Lists)*
