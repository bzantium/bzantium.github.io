---
layout: post
title: "[LeetCode 100] 19. Remove Nth Node From End of List"
subtitle: 
date: 2021-11-01
background: '/img/posts/code.jpg'
---

<h3>Problem:</h3>
<p>
Given the head of a linked list, remove the nth node from the end of the list and return its head.
</p>

```
Example 1:
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Example 2:
Input: head = [1], n = 1
Output: []

Example 3:
Input: head = [1,2], n = 1
Output: [1]
```

<br/>
<h3>Solution:</h3>
<p>1. 값 제거 후 리스트노드를 다시 만드는 방법</p>

<p>
노드를 돌면서 값을 모든 값을 리스트에 저장한 뒤 지정된 값을 제거하고 이를 기반으로 다시 리스트노드를 만들면 된다.
</p>

<p>2. 경우의 수를 나누어 해결하는 방법</p>

<p>
리스트노드의 길이가 1개일 경우 또는 n의 값이 길이와 동일한 경우를 먼저 처리해준 뒤, 뒤에서 (n+1)번 노드를 (n-1)번 노드에 연결해준다.
</p>


<p>3. Recursion 방법</p>

<p>
제거해야할 노드일 때만 다음 노드를 연결하고 그외의 것은 자신을 연결하는 방법을 Recursive하게 구현한다.
</p>

<br/>
<h3>Code:</h3>

<p>1. 값 제거 후 리스트노드를 다시 만드는 방법</p>

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        vals = []
        node = head
        
        while node is not None:
            vals.append(node.val)
            node = node.next
        
        vals.pop(len(vals) - n)
        
        node = ListNode(vals[0]) if vals else None
        head = node
        for i in range(len(vals) - 1):
            node.next = ListNode(vals[i + 1])
            node = node.next
```

<p>2. 경우의 수를 나누어 해결하는 방법</p>

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        length = 0
        node = head
        while node is not None:
            node = node.next
            length += 1
        
        if length == 1:
            return None
        
        if n == length:
            return head.next
            
        prev_node = head
        for _ in range(length - n - 1):
            prev_node = prev_node.next
        
        
        prev_node.next = prev_node.next.next
        return head
```

<p>3. Recursion 방법</p>

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        self.n = n
        head, _ = self.remove_nth_node(head)
        return head
    
    def remove_nth_node(self, head):
        if head.next is None:
            head_next = None
            cnt = 1
        else:
            head_next, cnt = self.remove_nth_node(head.next)
        
        if self.n == cnt:
            return head_next, cnt + 1
        else:
            return ListNode(val=head.val, next=head_next), cnt + 1
```
