---
comments: true
difficulty: Medium
rating: 1760
source: Weekly Contest 273 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [2121. Intervals Between Identical Elements](https://leetcode.com/problems/intervals-between-identical-elements)

[中文文档](/solution/2100-2199/2121.Intervals%20Between%20Identical%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>arr</code> có <code>n</code> phần tử.</p>

<p><strong>Khoảng cách</strong> giữa hai phần tử trong <code>arr</code> được định nghĩa là <strong>hiệu tuyệt đối</strong> giữa các chỉ số của chúng. Cụ thể hơn, <strong>khoảng cách</strong> giữa <code>arr[i]</code> và <code>arr[j]</code> là <code>|i - j|</code>.</p>

<p>Hãy trả về <em>một mảng</em> <code>intervals</code> <em>có độ dài</em> <code>n</code>, <em>trong đó</em> <code>intervals[i]</code> <em>là <strong>tổng khoảng cách</strong> giữa </em><code>arr[i]</code><em> và mọi phần tử trong </em><code>arr</code><em> có cùng giá trị với </em><code>arr[i]</code><em>.</em></p>

<p><strong>Lưu ý:</strong> <code>|x|</code> là giá trị tuyệt đối của <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,1,3,1,2,3,3]
<strong>Đầu ra:</strong> [4,2,7,2,4,4,5]
<strong>Giải thích:</strong>
- Chỉ số 0: Một phần tử 2 khác nằm ở chỉ số 4. |0 - 4| = 4
- Chỉ số 1: Một phần tử 1 khác nằm ở chỉ số 3. |1 - 3| = 2
- Chỉ số 2: Có thêm hai phần tử 3 ở các chỉ số 5 và 6. |2 - 5| + |2 - 6| = 7
- Chỉ số 3: Một phần tử 1 khác nằm ở chỉ số 1. |3 - 1| = 2
- Chỉ số 4: Một phần tử 2 khác nằm ở chỉ số 0. |4 - 0| = 4
- Chỉ số 5: Có thêm hai phần tử 3 ở các chỉ số 2 và 6. |5 - 2| + |5 - 6| = 4
- Chỉ số 6: Có thêm hai phần tử 3 ở các chỉ số 2 và 5. |6 - 2| + |6 - 5| = 5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [10,5,10,10]
<strong>Đầu ra:</strong> [5,0,3,4]
<strong>Giải thích:</strong>
- Chỉ số 0: Có thêm hai phần tử 10 ở các chỉ số 2 và 3. |0 - 2| + |0 - 3| = 5
- Chỉ số 1: Trong mảng chỉ có một phần tử 5, nên tổng khoảng cách đến các phần tử giống nó là 0.
- Chỉ số 2: Có thêm hai phần tử 10 ở các chỉ số 0 và 3. |2 - 0| + |2 - 3| = 3
- Chỉ số 3: Có thêm hai phần tử 10 ở các chỉ số 0 và 2. |3 - 0| + |3 - 2| = 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == arr.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>

<p><strong>Lưu ý:</strong> Câu hỏi này giống với <a href="https://leetcode.com/problems/sum-of-distances/description/" target="_blank"> 2615: Sum of Distances.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi $i$, ta cần tính $\sum_j |i-j|$ trên các chỉ số có cùng giá trị. Nếu nhóm theo giá trị rồi cộng từng cặp, độ phức tạp là $O(n^2)$ và không phù hợp với $n\le 10^5$.
>
> Các chỉ số của một giá trị được sắp xếp là $v_0,\ldots,v_{m-1}$. Khi di chuyển từ $v_{i-1}$ đến $v_i$ một đoạn $\Delta$, $i$ vị trí bên trái tăng thêm $\Delta$ còn $m-i$ vị trí bên phải giảm đi $\Delta$, nên có thể tính lần lượt tổng khoảng cách từ tổng khoảng cách đối với $v_0$.
>
> Gom các chỉ số theo giá trị, bắt đầu từ $\sum v-v_0\cdot m$, sau đó duyệt từng nhóm theo công thức truy hồi trên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getDistances(self, arr: List[int]) -> List[int]:
        d = defaultdict(list)
        n = len(arr)
        for i, v in enumerate(arr):
            d[v].append(i)
        ans = [0] * n
        for v in d.values():
            m = len(v)
            val = sum(v) - v[0] * m
            for i, p in enumerate(v):
                delta = v[i] - v[i - 1] if i >= 1 else 0
                val += i * delta - (m - i) * delta
                ans[p] = val
        return ans
```

#### Java

```java
class Solution {
    public long[] getDistances(int[] arr) {
        Map<Integer, List<Integer>> d = new HashMap<>();
        int n = arr.length;
        for (int i = 0; i < n; ++i) {
            d.computeIfAbsent(arr[i], k -> new ArrayList<>()).add(i);
        }
        long[] ans = new long[n];
        for (List<Integer> v : d.values()) {
            int m = v.size();
            long val = 0;
            for (int e : v) {
                val += e;
            }
            val -= (m * v.get(0));
            for (int i = 0; i < v.size(); ++i) {
                int delta = i >= 1 ? v.get(i) - v.get(i - 1) : 0;
                val += i * delta - (m - i) * delta;
                ans[v.get(i)] = val;
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
    vector<long long> getDistances(vector<int>& arr) {
        unordered_map<int, vector<int>> d;
        int n = arr.size();
        for (int i = 0; i < n; ++i) d[arr[i]].push_back(i);
        vector<long long> ans(n);
        for (auto& item : d) {
            auto& v = item.second;
            int m = v.size();
            long long val = 0;
            for (int e : v) val += e;
            val -= m * v[0];
            for (int i = 0; i < v.size(); ++i) {
                int delta = i >= 1 ? v[i] - v[i - 1] : 0;
                val += i * delta - (m - i) * delta;
                ans[v[i]] = val;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getDistances(arr []int) []int64 {
	d := make(map[int][]int)
	n := len(arr)
	for i, v := range arr {
		d[v] = append(d[v], i)
	}
	ans := make([]int64, n)
	for _, v := range d {
		m := len(v)
		val := 0
		for _, e := range v {
			val += e
		}
		val -= m * v[0]
		for i, p := range v {
			delta := 0
			if i >= 1 {
				delta = v[i] - v[i-1]
			}
			val += i*delta - (m-i)*delta
			ans[p] = int64(val)
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
