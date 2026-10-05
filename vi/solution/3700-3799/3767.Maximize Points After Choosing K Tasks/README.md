---
comments: true
difficulty: Medium
rating: 1703
source: Biweekly Contest 171 Q3
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3767. Maximize Points After Choosing K Tasks](https://leetcode.com/problems/maximize-points-after-choosing-k-tasks)

[中文文档](/solution/3700-3799/3767.Maximize%20Points%20After%20Choosing%20K%20Tasks/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>technique1</code> và <code>technique2</code>, mỗi mảng có độ dài <code>n</code>, trong đó <code>n</code> là số lượng task cần hoàn thành.</p>

<ul>
	<li>Nếu task thứ <code>i<sup>th</sup></code> được hoàn thành bằng kỹ thuật 1, bạn nhận được <code>technique1[i]</code> điểm.</li>
	<li>Nếu task đó được hoàn thành bằng kỹ thuật 2, bạn nhận được <code>technique2[i]</code> điểm.</li>
</ul>

<p>Bạn cũng được cho một số nguyên <code>k</code>, biểu thị số lượng <strong>tối thiểu</strong> task <strong>phải</strong> được hoàn thành bằng kỹ thuật 1.</p>

<p>Bạn <strong>phải</strong> hoàn thành <strong>ít nhất</strong> <code>k</code> task bằng kỹ thuật 1 (không nhất thiết phải là <code>k</code> task đầu tiên).</p>

<p>Các task còn lại có thể được hoàn thành bằng <strong>một trong hai</strong> kỹ thuật.</p>

<p>Hãy trả về một số nguyên biểu thị <strong>tổng điểm lớn nhất</strong> bạn có thể nhận được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">technique1 = [5,2,10], technique2 = [10,3,8], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta phải hoàn thành ít nhất <code>k = 2</code> task bằng <code>technique1</code>.</p>

<p>Chọn <code>technique1[1]</code> và <code>technique1[2]</code> (được hoàn thành bằng kỹ thuật 1), cùng với <code>technique2[0]</code> (được hoàn thành bằng kỹ thuật 2), ta nhận được số điểm lớn nhất: <code>2 + 10 + 10 = 22</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">technique1 = [10,20,30], technique2 = [5,15,25], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">60</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta phải hoàn thành ít nhất <code>k = 2</code> task bằng <code>technique1</code>.</p>

<p>Chọn kỹ thuật 1 cho tất cả task sẽ cho số điểm lớn nhất: <code>10 + 20 + 30 = 60</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">technique1 = [1,2,3], technique2 = [4,5,6], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì <code>k = 0</code>, ta không bắt buộc phải chọn task nào bằng <code>technique1</code>.</p>

<p>Chọn kỹ thuật 2 cho tất cả task sẽ cho số điểm lớn nhất: <code>4 + 5 + 6 = 15</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == technique1.length == technique2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= technique1[i], technique2​​​​​​​[i] &lt;= 10<sup>​​​​​​​5</sup></code></li>
	<li><code>0 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ít nhất $k$ task phải sử dụng kỹ thuật $1$; các task còn lại có thể sử dụng một trong hai kỹ thuật. Trước tiên, gán mọi task cho kỹ thuật $2$, sau đó chuyển $k$ hiệu lớn nhất $\textit{technique1}-\textit{technique2}$ sang kỹ thuật $1$, đồng thời chuyển thêm mọi hiệu không âm còn lại.

<!-- thinking:end -->

Trước tiên, ta gán tất cả task cho kỹ thuật 2, nên tổng điểm ban đầu là $\sum_{i=0}^{n-1} technique2[i]$.

Tiếp theo, ta tính mức tăng điểm của mỗi task nếu task đó được hoàn thành bằng kỹ thuật 1 thay vì kỹ thuật 2, ký hiệu là $\text{diff}[i] = technique1[i] - technique2[i]$. Ta sắp xếp các mức tăng này theo thứ tự giảm dần để thu được mảng chỉ số task đã sắp xếp $\text{idx}$.

Sau đó, ta chọn $k$ task đầu tiên để hoàn thành bằng kỹ thuật 1 và cộng mức chênh lệch điểm của chúng vào tổng điểm. Với các task còn lại, nếu sử dụng kỹ thuật 1 giúp tăng điểm (tức là $\text{diff}[i] \geq 0$), ta cũng chọn hoàn thành task đó bằng kỹ thuật 1.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng task.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPoints(self, technique1: List[int], technique2: List[int], k: int) -> int:
        n = len(technique1)
        idx = sorted(range(n), key=lambda i: -(technique1[i] - technique2[i]))
        ans = sum(technique2)
        for i in idx[:k]:
            ans -= technique2[i]
            ans += technique1[i]
        for i in idx[k:]:
            if technique1[i] >= technique2[i]:
                ans -= technique2[i]
                ans += technique1[i]
        return ans
```

#### Java

```java
class Solution {
    public long maxPoints(int[] technique1, int[] technique2, int k) {
        int n = technique1.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> technique1[j] - technique2[j] - (technique1[i] - technique2[i]));
        long ans = 0;
        for (int x : technique2) {
            ans += x;
        }
        for (int i = 0; i < k; i++) {
            int index = idx[i];
            ans -= technique2[index];
            ans += technique1[index];
        }
        for (int i = k; i < n; i++) {
            int index = idx[i];
            if (technique1[index] >= technique2[index]) {
                ans -= technique2[index];
                ans += technique1[index];
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxPoints(vector<int>& technique1, vector<int>& technique2, int k) {
        int n = technique1.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);

        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return (technique1[j] - technique2[j]) < (technique1[i] - technique2[i]);
        });

        long long ans = 0;
        for (int x : technique2) {
            ans += x;
        }

        for (int i = 0; i < k; i++) {
            int index = idx[i];
            ans -= technique2[index];
            ans += technique1[index];
        }

        for (int i = k; i < n; i++) {
            int index = idx[i];
            if (technique1[index] >= technique2[index]) {
                ans -= technique2[index];
                ans += technique1[index];
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxPoints(technique1 []int, technique2 []int, k int) int64 {
	n := len(technique1)
	idx := make([]int, n)
	for i := 0; i < n; i++ {
		idx[i] = i
	}

	sort.Slice(idx, func(i, j int) bool {
		return technique1[idx[j]]-technique2[idx[j]] < technique1[idx[i]]-technique2[idx[i]]
	})

	var ans int64
	for _, x := range technique2 {
		ans += int64(x)
	}

	for i := 0; i < k; i++ {
		index := idx[i]
		ans -= int64(technique2[index])
		ans += int64(technique1[index])
	}

	for i := k; i < n; i++ {
		index := idx[i]
		if technique1[index] >= technique2[index] {
			ans -= int64(technique2[index])
			ans += int64(technique1[index])
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maxPoints(technique1: number[], technique2: number[], k: number): number {
    const n = technique1.length;
    const idx = Array.from({ length: n }, (_, i) => i);

    idx.sort((i, j) => technique1[j] - technique2[j] - (technique1[i] - technique2[i]));

    let ans = technique2.reduce((sum, x) => sum + x, 0);

    for (let i = 0; i < k; i++) {
        const index = idx[i];
        ans -= technique2[index];
        ans += technique1[index];
    }

    for (let i = k; i < n; i++) {
        const index = idx[i];
        if (technique1[index] >= technique2[index]) {
            ans -= technique2[index];
            ans += technique1[index];
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
