---
comments: true
difficulty: Medium
rating: 1303
source: Weekly Contest 174 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1338. Reduce Array Size to The Half](https://leetcode.com/problems/reduce-array-size-to-the-half)

[中文文档](/solution/1300-1399/1338.Reduce%20Array%20Size%20to%20The%20Half/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho mảng số nguyên <code>arr</code>. Bạn có thể chọn một tập hợp các số nguyên rồi xóa mọi lần xuất hiện của các số đó trong mảng.</p>

<p>Hãy trả về <em>số phần tử ít nhất trong tập hợp sao cho <strong>ít nhất</strong> một nửa số phần tử của mảng bị xóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,3,3,3,5,5,5,2,2,7]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chọn {3,7} sẽ tạo ra mảng mới [5,5,5,2,2] có kích thước 5 (bằng một nửa kích thước mảng ban đầu).
Các tập hợp có kích thước 2 có thể chọn là {3,5},{3,2},{5,2}.
Không thể chọn tập hợp {2,7} vì mảng mới sẽ là [3,3,3,3,5,5,5], có kích thước lớn hơn một nửa kích thước mảng ban đầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [7,7,7,7,7,7]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Tập hợp duy nhất có thể chọn là {7}. Khi đó, mảng mới sẽ rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>arr.length</code> là số chẵn.</li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Xóa ít giá trị phân biệt nhất có thể sao cho nhiều nhất một nửa mảng còn lại. Chiến lược tham lam là chọn giá trị xuất hiện nhiều nhất trong số các giá trị còn lại. Sau khi đếm, ta cộng tần suất theo thứ tự giảm dần cho đến khi đạt ít nhất một nửa độ dài mảng; số giá trị đã chọn là đáp án.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $\textit{cnt}$ để đếm số lần xuất hiện của từng giá trị trong mảng $\textit{arr}$. Sau đó, sắp xếp các số trong $\textit{cnt}$ theo thứ tự giảm dần. Duyệt $\textit{cnt}$ từ lớn đến nhỏ, mỗi lần chọn giá trị hiện tại $x$ để tăng đáp án và cộng $x$ vào $m$. Nếu $m \geq \frac{n}{2}$, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSetSize(self, arr: List[int]) -> int:
        cnt = Counter(arr)
        ans = m = 0
        for _, v in cnt.most_common():
            m += v
            ans += 1
            if m * 2 >= len(arr):
                break
        return ans
```

#### Java

```java
class Solution {
    public int minSetSize(int[] arr) {
        int mx = 0;
        for (int x : arr) {
            mx = Math.max(mx, x);
        }
        int[] cnt = new int[mx + 1];
        for (int x : arr) {
            ++cnt[x];
        }
        Arrays.sort(cnt);
        int ans = 0;
        int m = 0;
        for (int i = mx;; --i) {
            if (cnt[i] > 0) {
                m += cnt[i];
                ++ans;
                if (m * 2 >= arr.length) {
                    return ans;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSetSize(vector<int>& arr) {
        int mx = ranges::max(arr);
        int cnt[mx + 1];
        memset(cnt, 0, sizeof(cnt));
        for (int& x : arr) {
            ++cnt[x];
        }
        sort(cnt, cnt + mx + 1, greater<int>());
        int ans = 0;
        int m = 0;
        for (int& x : cnt) {
            if (x) {
                m += x;
                ++ans;
                if (m * 2 >= arr.size()) {
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minSetSize(arr []int) (ans int) {
	mx := slices.Max(arr)
	cnt := make([]int, mx+1)
	for _, x := range arr {
		cnt[x]++
	}
	sort.Ints(cnt)
	for i, m := mx, 0; ; i-- {
		if cnt[i] > 0 {
			m += cnt[i]
			ans++
			if m >= len(arr)/2 {
				return
			}
		}
	}
}
```

#### TypeScript

```ts
function minSetSize(arr: number[]): number {
    const cnt = new Map<number, number>();
    for (const v of arr) {
        cnt.set(v, (cnt.get(v) ?? 0) + 1);
    }
    let [ans, m] = [0, 0];
    for (const v of Array.from(cnt.values()).sort((a, b) => b - a)) {
        m += v;
        ++ans;
        if (m * 2 >= arr.length) {
            break;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
