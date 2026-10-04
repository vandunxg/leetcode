---
comments: true
difficulty: Easy
rating: 1255
source: Weekly Contest 465 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3668. Restore Finishing Order](https://leetcode.com/problems/restore-finishing-order)

[中文文档](/solution/3600-3699/3668.Restore%20Finishing%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>order</code> có độ dài <code>n</code> và một mảng số nguyên <code>friends</code>.</p>

<ul>
	<li><code>order</code> chứa mỗi số nguyên từ 1 đến <code>n</code> <strong>đúng một lần</strong>, biểu thị ID của những người tham gia cuộc đua theo thứ tự <strong>về đích</strong> của họ.</li>
	<li><code>friends</code> chứa ID của bạn bè bạn trong cuộc đua theo thứ tự <strong>tăng dần</strong> nghiêm ngặt. Bảo đảm rằng mỗi ID trong friends đều xuất hiện trong mảng <code>order</code>.</li>
</ul>

<p>Trả về một mảng chứa ID của bạn bè bạn theo thứ tự <strong>về đích</strong> của họ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">order = [3,1,2,5,4], friends = [1,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,1,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thứ tự về đích là <code>[<u><strong>3</strong></u>, <u><strong>1</strong></u>, 2, 5, <u><strong>4</strong></u>]</code>. Do đó, thứ tự về đích của bạn bè bạn là <code>[3, 1, 4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">order = [1,4,5,3,2], friends = [2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thứ tự về đích là <code>[1, 4, <u><strong>5</strong></u>, 3, <u><strong>2</strong></u>]</code>. Do đó, thứ tự về đích của bạn bè bạn là <code>[5, 2]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == order.length &lt;= 100</code></li>
	<li><code>order</code> chứa mỗi số nguyên từ 1 đến <code>n</code> đúng một lần</li>
	<li><code>1 &lt;= friends.length &lt;= min(8, n)</code></li>
	<li><code>1 &lt;= friends[i] &lt;= n</code></li>
	<li><code>friends</code> tăng dần nghiêm ngặt</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{friends}$ là một tập con của $\textit{order}$ và phải được xuất ra theo thứ tự về đích. Vì $n\le 100$, ta có thể lập một bảng thứ hạng rồi sắp xếp.
>
> Đặt $d[x]=i$ với $\textit{order}$, sau đó sắp xếp $\textit{friends}$ theo $d[x]$.
>
> Khi đó, phép so sánh sẽ sử dụng thứ hạng về đích thay vì ID dạng số.

<!-- thinking:end -->

Trước tiên, ta xây dựng một ánh xạ từ mảng order để ghi lại vị trí về đích của mỗi ID. Sau đó, ta sắp xếp mảng friends dựa trên thứ tự về đích của những ID này trong mảng order.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng order.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def recoverOrder(self, order: List[int], friends: List[int]) -> List[int]:
        d = {x: i for i, x in enumerate(order)}
        return sorted(friends, key=lambda x: d[x])
```

#### Java

```java
class Solution {
    public int[] recoverOrder(int[] order, int[] friends) {
        int n = order.length;
        int[] d = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            d[order[i]] = i;
        }
        return Arrays.stream(friends)
            .boxed()
            .sorted((a, b) -> d[a] - d[b])
            .mapToInt(Integer::intValue)
            .toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> recoverOrder(vector<int>& order, vector<int>& friends) {
        int n = order.size();
        vector<int> d(n + 1);
        for (int i = 0; i < n; ++i) {
            d[order[i]] = i;
        }
        sort(friends.begin(), friends.end(), [&](int a, int b) {
            return d[a] < d[b];
        });
        return friends;
    }
};
```

#### Go

```go
func recoverOrder(order []int, friends []int) []int {
	n := len(order)
	d := make([]int, n+1)
	for i, x := range order {
		d[x] = i
	}
	sort.Slice(friends, func(i, j int) bool {
		return d[friends[i]] < d[friends[j]]
	})
	return friends
}
```

#### TypeScript

```ts
function recoverOrder(order: number[], friends: number[]): number[] {
    const n = order.length;
    const d: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        d[order[i]] = i;
    }
    return friends.sort((a, b) => d[a] - d[b]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
