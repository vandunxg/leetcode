---
comments: true
difficulty: Medium
tags:
    - Stack
    - Recursion
    - Linked List
    - Two Pointers
---

<!-- problem:start -->

# [1265. Print Immutable Linked List in Reverse 🔒](https://leetcode.com/problems/print-immutable-linked-list-in-reverse)

[中文文档](/solution/1200-1299/1265.Print%20Immutable%20Linked%20List%20in%20Reverse/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một linked list bất biến. Hãy in giá trị của các node theo thứ tự ngược bằng interface sau:</p>

<ul>
	<li><code>ImmutableListNode</code>: Interface của linked list bất biến; bạn được cung cấp head của danh sách.</li>
</ul>

<p>Bạn cần dùng các hàm sau để truy cập linked list (bạn <strong>không thể</strong> truy cập trực tiếp vào <code>ImmutableListNode</code>):</p>

<ul>
	<li><code>ImmutableListNode.printValue()</code>: In giá trị của node hiện tại.</li>
	<li><code>ImmutableListNode.getNext()</code>: Trả về node tiếp theo.</li>
</ul>

<p>Đầu vào chỉ được dùng để khởi tạo linked list bên trong. Bạn phải giải bài toán mà không chỉnh sửa linked list. Nói cách khác, chỉ được thao tác với danh sách thông qua các API đã nêu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> head = [1,2,3,4]
<strong>Output:</strong> [4,3,2,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> head = [0,-4,-1,3,-5]
<strong>Output:</strong> [-5,3,-1,-4,0]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> head = [-2,0,6,4,4,-6]
<strong>Output:</strong> [-6,4,4,6,0,-2]
</pre>

<ul>
</ul>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Độ dài linked list nằm trong khoảng <code>[1, 1000]</code>.</li>
	<li>Giá trị của mỗi node trong linked list nằm trong khoảng <code>[-1000, 1000]</code>.</li>
</ul>

<p>&nbsp;</p>

<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán với:</p>

<ul>
	<li>Độ phức tạp không gian hằng số?</li>
	<li>Độ phức tạp thời gian tuyến tính và độ phức tạp không gian nhỏ hơn tuyến tính?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Linked list là bất biến: ta chỉ có thể gọi $getNext$ và $printValue$, độ dài tối đa $1000$. Muốn in theo thứ tự ngược, cần đến node kế tiếp trước khi in node hiện tại. Đệ quy rồi in sau lời gọi sẽ cho thứ tự từ cuối lên đầu; call stack lưu các node đã đi qua.

<!-- thinking:end -->

Ta có thể dùng đệ quy để in linked list theo thứ tự ngược. Trong hàm, trước tiên kiểm tra node hiện tại có null hay không. Nếu không, ta lấy node tiếp theo, gọi đệ quy hàm này, rồi cuối cùng in giá trị của node hiện tại.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài linked list.

<!-- tabs:start -->

#### Python3

```python
# """
# This is the ImmutableListNode's API interface.
# You should not implement it, or speculate about its implementation.
# """
# class ImmutableListNode:
#     def printValue(self) -> None: # print the value of this node.
#     def getNext(self) -> 'ImmutableListNode': # return the next node.


class Solution:
    def printLinkedListInReverse(self, head: 'ImmutableListNode') -> None:
        if head:
            self.printLinkedListInReverse(head.getNext())
            head.printValue()
```

#### Java

```java
/**
 * // This is the ImmutableListNode's API interface.
 * // You should not implement it, or speculate about its implementation.
 * interface ImmutableListNode {
 *     public void printValue(); // print the value of this node.
 *     public ImmutableListNode getNext(); // return the next node.
 * };
 */

class Solution {
    public void printLinkedListInReverse(ImmutableListNode head) {
        if (head != null) {
            printLinkedListInReverse(head.getNext());
            head.printValue();
        }
    }
}
```

#### C++

```cpp
/**
 * // This is the ImmutableListNode's API interface.
 * // You should not implement it, or speculate about its implementation.
 * class ImmutableListNode {
 * public:
 *    void printValue(); // print the value of the node.
 *    ImmutableListNode* getNext(); // return the next node.
 * };
 */

class Solution {
public:
    void printLinkedListInReverse(ImmutableListNode* head) {
        if (head) {
            printLinkedListInReverse(head->getNext());
            head->printValue();
        }
    }
};
```

#### Go

```go
/*   Below is the interface for ImmutableListNode, which is already defined for you.
 *
 *   type ImmutableListNode struct {
 *
 *   }
 *
 *   func (this *ImmutableListNode) getNext() ImmutableListNode {
 *		// return the next node.
 *   }
 *
 *   func (this *ImmutableListNode) printValue() {
 *		// print the value of this node.
 *   }
 */

func printLinkedListInReverse(head ImmutableListNode) {
	if head != nil {
		printLinkedListInReverse(head.getNext())
		head.printValue()
	}
}
```

#### TypeScript

```ts
/**
 * // This is the ImmutableListNode's API interface.
 * // You should not implement it, or speculate about its implementation
 * class ImmutableListNode {
 *      printValue() {}
 *
 *      getNext(): ImmutableListNode {}
 * }
 */

function printLinkedListInReverse(head: ImmutableListNode) {
    if (head) {
        printLinkedListInReverse(head.next);
        head.printValue();
    }
}
```

#### C#

```cs
/**
 * // This is the ImmutableListNode's API interface.
 * // You should not implement it, or speculate about its implementation.
 * class ImmutableListNode {
 *     public void PrintValue(); // print the value of this node.
 *     public ImmutableListNode GetNext(); // return the next node.
 * }
 */

public class Solution {
    public void PrintLinkedListInReverse(ImmutableListNode head) {
        if (head != null) {
            PrintLinkedListInReverse(head.GetNext());
            head.PrintValue();
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
