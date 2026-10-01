---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - Binary Search Tree
    - Recursion
    - Array
    - Binary Tree
    - Monotonic Stack
---

<!-- problem:start -->

# [255. Verify Preorder Sequence in Binary Search Tree 🔒](https://leetcode.com/problems/verify-preorder-sequence-in-binary-search-tree)

[中文文档](/solution/0200-0299/0255.Verify%20Preorder%20Sequence%20in%20Binary%20Search%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>không trùng lặp</strong> <code>preorder</code>. Hãy trả về <code>true</code> <em>nếu đây là thứ tự duyệt preorder hợp lệ của một binary search tree</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0255.Verify%20Preorder%20Sequence%20in%20Binary%20Search%20Tree/images/preorder-tree.jpg" style="width: 292px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> preorder = [5,2,1,3,6]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> preorder = [5,2,6,1,3]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= preorder.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= preorder[i] &lt;= 10<sup>4</sup></code></li>
	<li>Tất cả phần tử trong <code>preorder</code> đều <strong>không trùng lặp</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này chỉ với độ phức tạp không gian hằng số không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Dựng lại BST sẽ tốn công hơn mức cần thiết. Preorder lần lượt thăm root, cây con trái rồi cây con phải; một stack giảm dần lưu các node chưa chuyển sang cây con phải.
>
> Nếu gặp giá trị nhỏ hơn cận dưới gần nhất đã lấy ra khỏi stack thì thứ tự không hợp lệ. Nếu không, lấy ra mọi giá trị nhỏ hơn ở đỉnh stack để cập nhật cận dưới, rồi push giá trị hiện tại vào stack.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def verifyPreorder(self, preorder: List[int]) -> bool:
        stk = []
        last = -inf
        for x in preorder:
            if x < last:
                return False
            while stk and stk[-1] < x:
                last = stk.pop()
            stk.append(x)
        return True
```

#### Java

```java
class Solution {
    public boolean verifyPreorder(int[] preorder) {
        Deque<Integer> stk = new ArrayDeque<>();
        int last = Integer.MIN_VALUE;
        for (int x : preorder) {
            if (x < last) {
                return false;
            }
            while (!stk.isEmpty() && stk.peek() < x) {
                last = stk.poll();
            }
            stk.push(x);
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool verifyPreorder(vector<int>& preorder) {
        stack<int> stk;
        int last = INT_MIN;
        for (int x : preorder) {
            if (x < last) return false;
            while (!stk.empty() && stk.top() < x) {
                last = stk.top();
                stk.pop();
            }
            stk.push(x);
        }
        return true;
    }
};
```

#### Go

```go
func verifyPreorder(preorder []int) bool {
	var stk []int
	last := math.MinInt32
	for _, x := range preorder {
		if x < last {
			return false
		}
		for len(stk) > 0 && stk[len(stk)-1] < x {
			last = stk[len(stk)-1]
			stk = stk[0 : len(stk)-1]
		}
		stk = append(stk, x)
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
