---
comments: true
difficulty: Medium
tags:
    - Linked List
    - Math
    - Two Pointers
---

<!-- problem:start -->

# [1634. Add Two Polynomials Represented as Linked Lists 🔒](https://leetcode.com/problems/add-two-polynomials-represented-as-linked-lists)

[中文文档](/solution/1600-1699/1634.Add%20Two%20Polynomials%20Represented%20as%20Linked%20Lists/README.md)

## Mô tả

<!-- description:start -->

<p>Danh sách liên kết đa thức là một loại danh sách liên kết đặc biệt trong đó mỗi nút biểu thị một thuật ngữ trong biểu thức đa thức.</p>

<p>Mỗi nút có ba thuộc tính:</p>

<ul>
	<li><code>coefficient</code>: một số nguyên biểu thị số nhân của số hạng. Hệ số của kỳ hạn<code><strong>9</strong>x<sup>4</sup></code>là<code>9</code>.</li>
	<li><code>power</code>: một số nguyên biểu thị số mũ. Sức mạnh của thuật ngữ<code>9x<strong><sup>4</sup></strong></code>là<code>4</code>.</li>
	<li><code>next</code>: một con trỏ tới nút tiếp theo trong danh sách, hoặc<code>null</code>nếu đó là nút cuối cùng của danh sách.</li>
</ul>

<p>Ví dụ, đa thức<code>5x<sup>3</sup> + 4x - 7</code>được biểu diễn bằng danh sách liên kết đa thức minh họa dưới đây:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1634.Add%20Two%20Polynomials%20Represented%20as%20Linked%20Lists/images/polynomial2.png" style="width: 500px; height: 91px;" /></p>

<p>Danh sách liên kết đa thức phải ở dạng chuẩn: đa thức phải ở dạng<strong>strictly</strong>thứ tự giảm dần của nó<code>power</code>giá trị. Ngoài ra, các điều khoản với một<code>coefficient</code>của<code>0</code>được bỏ qua.</p>

<p>Cho hai đầu danh sách liên kết đa thức,<code>poly1</code>Và<code>poly2</code>, cộng các đa thức lại với nhau và trả về<em>the head of the sum of the polynomials</em>.</p>

<p><strong><code>PolyNode</code> format:</strong></p>

<p>Định dạng đầu vào/đầu ra là một danh sách<code>n</code>các nút, trong đó mỗi nút được biểu diễn dưới dạng<code>[coefficient, power]</code>. Ví dụ, đa thức<code>5x<sup>3</sup> + 4x - 7</code>sẽ được biểu diễn dưới dạng:<code>[[5,3],[4,1],[-7,0]]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1634.Add%20Two%20Polynomials%20Represented%20as%20Linked%20Lists/images/ex1.png" style="width: 600px; height: 322px;" /></p>

<pre>
<strong>Input:</strong> poly1 = [[1,1]], poly2 = [[1,0]]
<strong>Output:</strong> [[1,1],[1,0]]
<strong>Explanation:</strong> poly1 = x. poly2 = 1. The sum is x + 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> poly1 = [[2,2],[4,1],[3,0]], poly2 = [[3,2],[-4,1],[-1,0]]
<strong>Output:</strong> [[5,2],[2,0]]
<strong>Explanation:</strong> poly1 = 2x<sup>2</sup> + 4x + 3. poly2 = 3x<sup>2</sup> - 4x - 1. The sum is 5x<sup>2</sup> + 2. Notice that we omit the &quot;0x&quot; term.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> poly1 = [[1,2]], poly2 = [[-1,2]]
<strong>Output:</strong> []
<strong>Explanation:</strong> The sum is 0. We return an empty list.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= PolyNode.coefficient &lt;= 10<sup>9</sup></code></li>
	<li><code>PolyNode.coefficient != 0</code></li>
	<li><code>0&nbsp;&lt;= PolyNode.power &lt;= 10<sup>9</sup></code></li>
	<li><code>PolyNode.power &gt; PolyNode.next.power</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Suy nghĩ**
>
> Cả hai danh sách đều giảm dần theo lũy thừa, do đó việc hợp nhất cũng giống như hợp nhất các danh sách đã sắp xếp: lấy lũy thừa lớn hơn hoặc thêm hệ số khi lũy thừa khớp.
>
> Tổng bằng 0 phải bỏ qua thuật ngữ. Khi một danh sách kết thúc, hãy nối thêm phần còn lại.
>
> Nút giả giữ phần đầu giống hệt phần giữa.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for polynomial singly-linked list.
# class PolyNode:
#     def __init__(self, x=0, y=0, next=None):
#         self.coefficient = x
#         self.power = y
#         self.next = next


class Solution:
    def addPoly(self, poly1: "PolyNode", poly2: "PolyNode") -> "PolyNode":
        dummy = curr = PolyNode()
        while poly1 and poly2:
            if poly1.power > poly2.power:
                curr.next = poly1
                poly1 = poly1.next
                curr = curr.next
            elif poly1.power < poly2.power:
                curr.next = poly2
                poly2 = poly2.next
                curr = curr.next
            else:
                if c := poly1.coefficient + poly2.coefficient:
                    curr.next = PolyNode(c, poly1.power)
                    curr = curr.next
                poly1 = poly1.next
                poly2 = poly2.next
        curr.next = poly1 or poly2
        return dummy.next
```

#### Java

```java
/**
 * Definition for polynomial singly-linked list.
 * class PolyNode {
 *     int coefficient, power;
 *     PolyNode next = null;

 *     PolyNode() {}
 *     PolyNode(int x, int y) { this.coefficient = x; this.power = y; }
 *     PolyNode(int x, int y, PolyNode next) { this.coefficient = x; this.power = y; this.next =
 next; }
 * }
 */

class Solution {
    public PolyNode addPoly(PolyNode poly1, PolyNode poly2) {
        PolyNode dummy = new PolyNode();
        PolyNode curr = dummy;
        while (poly1 != null && poly2 != null) {
            if (poly1.power > poly2.power) {
                curr.next = poly1;
                poly1 = poly1.next;
                curr = curr.next;
            } else if (poly1.power < poly2.power) {
                curr.next = poly2;
                poly2 = poly2.next;
                curr = curr.next;
            } else {
                int c = poly1.coefficient + poly2.coefficient;
                if (c != 0) {
                    curr.next = new PolyNode(c, poly1.power);
                    curr = curr.next;
                }
                poly1 = poly1.next;
                poly2 = poly2.next;
            }
        }
        if (poly1 == null) {
            curr.next = poly2;
        }
        if (poly2 == null) {
            curr.next = poly1;
        }
        return dummy.next;
    }
}
```

#### C++

```cpp
/**
 * Definition for polynomial singly-linked list->
 * struct PolyNode {
 *     int coefficient, power;
 *     PolyNode *next;
 *     PolyNode(): coefficient(0), power(0), next(nullptr) {};
 *     PolyNode(int x, int y): coefficient(x), power(y), next(nullptr) {};
 *     PolyNode(int x, int y, PolyNode* next): coefficient(x), power(y), next(next) {};
 * };
 */

class Solution {
public:
    PolyNode* addPoly(PolyNode* poly1, PolyNode* poly2) {
        PolyNode* dummy = new PolyNode();
        PolyNode* curr = dummy;
        while (poly1 && poly2) {
            if (poly1->power > poly2->power) {
                curr->next = poly1;
                poly1 = poly1->next;
                curr = curr->next;
            } else if (poly1->power < poly2->power) {
                curr->next = poly2;
                poly2 = poly2->next;
                curr = curr->next;
            } else {
                int c = poly1->coefficient + poly2->coefficient;
                if (c != 0) {
                    curr->next = new PolyNode(c, poly1->power);
                    curr = curr->next;
                }
                poly1 = poly1->next;
                poly2 = poly2->next;
            }
        }
        if (!poly1) {
            curr->next = poly2;
        }
        if (!poly2) {
            curr->next = poly1;
        }
        return dummy->next;
    }
};
```

#### JavaScript

```js
/**
 * Definition for polynomial singly-linked list.
 * function PolyNode(x=0, y=0, next=null) {
 *     this.coefficient = x;
 *     this.power = y;
 *     this.next = next;
 * }
 */

/**
 * @param {PolyNode} poly1
 * @param {PolyNode} poly2
 * @return {PolyNode}
 */
var addPoly = function (poly1, poly2) {
    const dummy = new PolyNode();
    let curr = dummy;
    while (poly1 && poly2) {
        if (poly1.power > poly2.power) {
            curr.next = poly1;
            poly1 = poly1.next;
            curr = curr.next;
        } else if (poly1.power < poly2.power) {
            curr.next = poly2;
            poly2 = poly2.next;
            curr = curr.next;
        } else {
            const c = poly1.coefficient + poly2.coefficient;
            if (c != 0) {
                curr.next = new PolyNode(c, poly1.power);
                curr = curr.next;
            }
            poly1 = poly1.next;
            poly2 = poly2.next;
        }
    }
    curr.next = poly1 || poly2;
    return dummy.next;
};
```

#### C#

```cs
/**
 * Definition for polynomial singly-linked list.
 * public class PolyNode {
 *     public int coefficient, power;
 *     public PolyNode next;
 *
 *     public PolyNode(int x=0, int y=0, PolyNode next=null) {
 *         this.coefficient = x;
 *         this.power = y;
 *         this.next = next;
 *     }
 * }
 */

public class Solution {
    public PolyNode AddPoly(PolyNode poly1, PolyNode poly2) {
        PolyNode dummy = new PolyNode();
        PolyNode curr = dummy;
        while (poly1 != null && poly2 != null) {
            if (poly1.power > poly2.power) {
                curr.next = poly1;
                poly1 = poly1.next;
                curr = curr.next;
            } else if (poly1.power < poly2.power) {
                curr.next = poly2;
                poly2 = poly2.next;
                curr = curr.next;
            } else {
                int c = poly1.coefficient + poly2.coefficient;
                if (c != 0) {
                    curr.next = new PolyNode(c, poly1.power);
                    curr = curr.next;
                }
                poly1 = poly1.next;
                poly2 = poly2.next;
            }
        }
        if (poly1 == null) {
            curr.next = poly2;
        }
        if (poly2 == null) {
            curr.next = poly1;
        }
        return dummy.next;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
