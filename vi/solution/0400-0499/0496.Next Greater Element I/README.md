---
comments: true
difficulty: Easy
tags:
    - Stack
    - Array
    - Hash Table
    - Monotonic Stack
---

<!-- problem:start -->

# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i)

[中文文档](/solution/0400-0499/0496.Next%20Greater%20Element%20I/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Phần tử lớn hơn tiếp theo</strong> của một phần tử <code>x</code> trong mảng là phần tử <strong>đầu tiên lớn hơn</strong> nằm <strong>bên phải</strong> <code>x</code> trong cùng mảng.</p>

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> <strong>đánh chỉ số từ 0 và không chứa phần tử trùng nhau</strong>, trong đó <code>nums1</code> là tập con của <code>nums2</code>.</p>

<p>Với mỗi <code>0 &lt;= i &lt; nums1.length</code>, hãy tìm chỉ số <code>j</code> sao cho <code>nums1[i] == nums2[j]</code>, rồi xác định <strong>phần tử lớn hơn tiếp theo</strong> của <code>nums2[j]</code> trong <code>nums2</code>. Nếu không có phần tử lớn hơn tiếp theo, kết quả của truy vấn này là <code>-1</code>.</p>

<p>Hãy trả về <em>mảng </em><code>ans</code><em> có độ dài </em><code>nums1.length</code><em>, sao cho </em><code>ans[i]</code><em> là <strong>phần tử lớn hơn tiếp theo</strong> như mô tả ở trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [4,1,2], nums2 = [1,3,4,2]
<strong>Đầu ra:</strong> [-1,3,-1]
<strong>Giải thích:</strong> Phần tử lớn hơn tiếp theo cho từng giá trị trong nums1 như sau:
- 4 được gạch chân trong nums2 = [1,3,<u>4</u>,2]. Không có phần tử lớn hơn tiếp theo, nên kết quả là -1.
- 1 được gạch chân trong nums2 = [<u>1</u>,3,4,2]. Phần tử lớn hơn tiếp theo là 3.
- 2 được gạch chân trong nums2 = [1,3,4,<u>2</u>]. Không có phần tử lớn hơn tiếp theo, nên kết quả là -1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,4], nums2 = [1,2,3,4]
<strong>Đầu ra:</strong> [3,-1]
<strong>Giải thích:</strong> Phần tử lớn hơn tiếp theo cho từng giá trị trong nums1 như sau:
- 2 được gạch chân trong nums2 = [1,<u>2</u>,3,4]. Phần tử lớn hơn tiếp theo là 3.
- 4 được gạch chân trong nums2 = [1,2,3,<u>4</u>]. Không có phần tử lớn hơn tiếp theo, nên kết quả là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length &lt;= nums2.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 10<sup>4</sup></code></li>
	<li>Tất cả số nguyên trong <code>nums1</code> và <code>nums2</code> đều <strong>khác nhau</strong>.</li>
	<li>Tất cả số nguyên trong <code>nums1</code> cũng xuất hiện trong <code>nums2</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm lời giải với độ phức tạp <code>O(nums1.length + nums2.length)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> $nums1$ là tập con của $nums2$; ta cần tìm phần tử lớn hơn tiếp theo trong $nums2$ cho từng giá trị. Quét sang phải từ mỗi giá trị trong $nums1$ sẽ mất $O(nm)$.
>
> Duyệt $nums2$ từ phải sang trái bằng một stack giảm dần: sau khi pop các phần tử nhỏ hơn ở đỉnh, phần tử mới ở đỉnh là giá trị lớn hơn tiếp theo; lưu giá trị này vào map. Sau đó tra cứu các giá trị trong $nums1$.
>
> Mỗi giá trị chỉ được đưa vào và lấy ra khỏi stack một lần. Tạo map từ $nums2$ trước giúp tránh quét mảng này cho từng truy vấn.

<!-- thinking:end -->

Ta có thể duyệt mảng $\textit{nums2}$ từ phải sang trái, duy trì stack $\textit{stk}$ có thứ tự tăng dần từ đỉnh xuống đáy. Ta dùng hash table $\textit{d}$ để lưu phần tử lớn hơn tiếp theo cho mỗi phần tử.

Khi gặp phần tử $x$, nếu stack không rỗng và phần tử ở đỉnh nhỏ hơn $x$, ta tiếp tục pop các phần tử ở đỉnh cho đến khi stack rỗng hoặc phần tử ở đỉnh lớn hơn hoặc bằng $x$. Lúc này, nếu stack không rỗng thì phần tử ở đỉnh là phần tử lớn hơn tiếp theo của $x$. Nếu không, $x$ không có phần tử lớn hơn tiếp theo.

Cuối cùng, ta duyệt mảng $\textit{nums1}$ và dùng hash table $\textit{d}$ để lấy đáp án.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của các mảng $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        stk = []
        d = {}
        for x in nums2[::-1]:
            while stk and stk[-1] < x:
                stk.pop()
            if stk:
                d[x] = stk[-1]
            stk.append(x)
        return [d.get(x, -1) for x in nums1]
```

#### Java

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Deque<Integer> stk = new ArrayDeque<>();
        int m = nums1.length, n = nums2.length;
        Map<Integer, Integer> d = new HashMap(n);
        for (int i = n - 1; i >= 0; --i) {
            int x = nums2[i];
            while (!stk.isEmpty() && stk.peek() < x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                d.put(x, stk.peek());
            }
            stk.push(x);
        }
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            ans[i] = d.getOrDefault(nums1[i], -1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> stk;
        unordered_map<int, int> d;
        ranges::reverse(nums2);
        for (int x : nums2) {
            while (!stk.empty() && stk.top() < x) {
                stk.pop();
            }
            if (!stk.empty()) {
                d[x] = stk.top();
            }
            stk.push(x);
        }
        vector<int> ans;
        for (int x : nums1) {
            ans.push_back(d.contains(x) ? d[x] : -1);
        }
        return ans;
    }
};
```

#### Go

```go
func nextGreaterElement(nums1 []int, nums2 []int) (ans []int) {
	stk := []int{}
	d := map[int]int{}
	for i := len(nums2) - 1; i >= 0; i-- {
		x := nums2[i]
		for len(stk) > 0 && stk[len(stk)-1] < x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			d[x] = stk[len(stk)-1]
		}
		stk = append(stk, x)
	}
	for _, x := range nums1 {
		if v, ok := d[x]; ok {
			ans = append(ans, v)
		} else {
			ans = append(ans, -1)
		}
	}
	return
}
```

#### TypeScript

```ts
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
    const stk: number[] = [];
    const d: Record<number, number> = {};
    for (const x of nums2.reverse()) {
        while (stk.length && stk.at(-1)! < x) {
            stk.pop();
        }
        d[x] = stk.length ? stk.at(-1)! : -1;
        stk.push(x);
    }
    return nums1.map(x => d[x]);
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn next_greater_element(nums1: Vec<i32>, nums2: Vec<i32>) -> Vec<i32> {
        let mut stk = Vec::new();
        let mut d = HashMap::new();
        for &x in nums2.iter().rev() {
            while let Some(&top) = stk.last() {
                if top <= x {
                    stk.pop();
                } else {
                    break;
                }
            }
            if let Some(&top) = stk.last() {
                d.insert(x, top);
            }
            stk.push(x);
        }

        nums1
            .into_iter()
            .map(|x| *d.get(&x).unwrap_or(&-1))
            .collect()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var nextGreaterElement = function (nums1, nums2) {
    const stk = [];
    const d = {};
    for (const x of nums2.reverse()) {
        while (stk.length && stk.at(-1) < x) {
            stk.pop();
        }
        d[x] = stk.length ? stk.at(-1) : -1;
        stk.push(x);
    }
    return nums1.map(x => d[x]);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
