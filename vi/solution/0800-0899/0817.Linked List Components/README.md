---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Linked List
---

<!-- problem:start -->

# [817. Linked List Components](https://leetcode.com/problems/linked-list-components)

[中文文档](/solution/0800-0899/0817.Linked%20List%20Components/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>head</code> của một linked list chứa các giá trị nguyên không trùng nhau và mảng số nguyên <code>nums</code> là tập con các giá trị trong linked list.</p>

<p>Trả về số <strong>component liên thông</strong> trong <code>nums</code>. Một component liên thông là một dãy node <strong>liên tiếp</strong> dài nhất và không rỗng trong linked list, sao cho giá trị của mọi node đều thuộc nums.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0817.Linked%20List%20Components/images/lc-linkedlistcom1.jpg" style="width: 424px; height: 65px;" />
<pre>
<strong>Đầu vào:</strong> head = [0,1,2,3], nums = [0,1,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 0 và 1 liên thông với nhau, nên [0, 1] và [3] là hai component liên thông.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0817.Linked%20List%20Components/images/lc-linkedlistcom2.jpg" style="width: 544px; height: 65px;" />
<pre>
<strong>Đầu vào:</strong> head = [0,1,2,3,4], nums = [0,3,1,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 0 và 1 liên thông với nhau, 3 và 4 cũng liên thông với nhau, nên [0, 1] và [3, 4] là hai component liên thông.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Linked list có <code>n</code> node.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= Node.val &lt; n</code></li>
	<li>Các giá trị <code>Node.val</code> đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;= nums.length &lt;= n</code></li>
	<li><code>0 &lt;= nums[i] &lt; n</code></li>
	<li>Các giá trị trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một component là một đoạn liên tiếp dài nhất gồm các giá trị thuộc $nums$ trong linked list. Vì $n\le 10^4$, chỉ cần tra cứu trong set và duyệt danh sách một lượt; không cần dựng graph.
>
> Bỏ qua các node không thuộc set. Mỗi khi gặp đầu một đoạn gồm các node thuộc set, tăng số component lên 1 rồi duyệt qua hết đoạn đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def numComponents(self, head: Optional[ListNode], nums: List[int]) -> int:
        ans = 0
        s = set(nums)
        while head:
            while head and head.val not in s:
                head = head.next
            ans += head is not None
            while head and head.val in s:
                head = head.next
        return ans
```

#### Java

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int numComponents(ListNode head, int[] nums) {
        int ans = 0;
        Set<Integer> s = new HashSet<>();
        for (int v : nums) {
            s.add(v);
        }
        while (head != null) {
            while (head != null && !s.contains(head.val)) {
                head = head.next;
            }
            ans += head != null ? 1 : 0;
            while (head != null && s.contains(head.val)) {
                head = head.next;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int numComponents(ListNode* head, vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        int ans = 0;
        while (head) {
            while (head && !s.count(head->val)) head = head->next;
            ans += head != nullptr;
            while (head && s.count(head->val)) head = head->next;
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func numComponents(head *ListNode, nums []int) int {
	s := map[int]bool{}
	for _, v := range nums {
		s[v] = true
	}
	ans := 0
	for head != nil {
		for head != nil && !s[head.Val] {
			head = head.Next
		}
		if head != nil {
			ans++
		}
		for head != nil && s[head.Val] {
			head = head.Next
		}
	}
	return ans
}
```

#### TypeScript

```ts
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     val: number
 *     next: ListNode | null
 *     constructor(val?: number, next?: ListNode | null) {
 *         this.val = (val===undefined ? 0 : val)
 *         this.next = (next===undefined ? null : next)
 *     }
 * }
 */

function numComponents(head: ListNode | null, nums: number[]): number {
    const set = new Set<number>(nums);
    let res = 0;
    let cur = head;
    let inSet = false;
    while (cur != null) {
        if (set.has(cur.val)) {
            if (!inSet) {
                inSet = true;
                res++;
            }
        } else {
            inSet = false;
        }
        cur = cur.next;
    }
    return res;
}
```

#### Rust

```rust
// Definition for singly-linked list.
// #[derive(PartialEq, Eq, Clone, Debug)]
// pub struct ListNode {
//   pub val: i32,
//   pub next: Option<Box<ListNode>>
// }
//
// impl ListNode {
//   #[inline]
//   fn new(val: i32) -> Self {
//     ListNode {
//       next: None,
//       val
//     }
//   }
// }
use std::collections::HashSet;
impl Solution {
    pub fn num_components(head: Option<Box<ListNode>>, nums: Vec<i32>) -> i32 {
        let set = nums.into_iter().collect::<HashSet<i32>>();
        let mut res = 0;
        let mut in_set = false;
        let mut cur = &head;
        while let Some(node) = cur {
            if set.contains(&node.val) {
                if !in_set {
                    in_set = true;
                    res += 1;
                }
            } else {
                in_set = false;
            }
            cur = &node.next;
        }
        res
    }
}
```

#### JavaScript

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @param {number[]} nums
 * @return {number}
 */
var numComponents = function (head, nums) {
    const s = new Set(nums);
    let ans = 0;
    while (head) {
        while (head && !s.has(head.val)) {
            head = head.next;
        }
        ans += head != null;
        while (head && s.has(head.val)) {
            head = head.next;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
