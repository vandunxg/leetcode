---
comments: true
difficulty: Medium
rating: 1850
source: Biweekly Contest 28 Q3
tags:
    - Array
    - Hash Table
    - Binary Search
    - Dynamic Programming
    - Sliding Window
---

<!-- problem:start -->

# [1477. Find Two Non-overlapping Sub-arrays Each With Target Sum](https://leetcode.com/problems/find-two-non-overlapping-sub-arrays-each-with-target-sum)

[中文文档](/solution/1400-1499/1477.Find%20Two%20Non-overlapping%20Sub-arrays%20Each%20With%20Target%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> và một số nguyên <code>target</code>.</p>

<p>Hãy tìm <strong>hai mảng con không giao nhau</strong> của <code>arr</code>, mỗi mảng có tổng bằng <code>target</code>. Có thể có nhiều đáp án, vì vậy cần tìm đáp án có <strong>tổng độ dài của hai mảng con là nhỏ nhất</strong>.</p>

<p>Trả về <em>tổng độ dài nhỏ nhất</em> của hai mảng con cần tìm, hoặc trả về <code>-1</code> nếu không thể tìm được hai mảng con như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,2,2,4,3], target = 3
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Chỉ có hai mảng con có tổng = 3 ([3] và [3]). Tổng độ dài của chúng là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [7,3,4,7], target = 7
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Mặc dù có ba mảng con không giao nhau có tổng = 7 ([7], [3,4] và [7]), ta sẽ chọn mảng con thứ nhất và thứ ba vì tổng độ dài của chúng là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [4,3,2,6,2,3,4], target = 6
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Chỉ có một mảng con có tổng = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= arr[i] &lt;= 1000</code></li>
    <li><code>1 &lt;= target &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Prefix Sum + Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Cách làm trực tiếp là liệt kê mọi mảng con có tổng bằng $target$, sau đó ghép từng cặp và kiểm tra xem chúng có giao nhau hay không. Với $n \le 10^5$, số lượng mảng con là bậc hai, nên cách này không phù hợp.
>
> Khi cố định đoạn bên phải, đoạn bên trái phải nằm hoàn toàn trong phần prefix trước nó, và ta chỉ cần mảng con hợp lệ ngắn nhất trong prefix đó. Vì vậy, cần duy trì đáp án "đoạn hợp lệ ngắn nhất trong prefix tính đến hiện tại".
>
> Vì mọi giá trị đều dương, các prefix sum tăng nghiêm ngặt và không trùng nhau. Do đó, một hash map ánh xạ prefix sum với chỉ số sẽ tìm được trong thời gian hằng số đoạn duy nhất $[j+1,i]$ kết thúc tại vị trí hiện tại và có tổng bằng $target$.
>
> Ta duyệt từ trái sang phải, đồng thời duy trì $f[i]$, là mảng con như vậy ngắn nhất trong $i$ phần tử đầu tiên. Khi xuất hiện $[j+1,i]$, cộng $f[j]$ vào độ dài hiện tại để cập nhật đáp án, sau đó đặt $f[i]=\min(f[i-1], i-j)$. Phần bên trái luôn nằm trước đoạn hiện tại nên hai đoạn không bao giờ giao nhau.

<!-- thinking:end -->

Ta dùng một hash table $d$ để ghi lại chỉ số của mỗi prefix sum, ban đầu $d[0]=0$.

Định nghĩa $f[i]$ là độ dài nhỏ nhất của một mảng con có tổng bằng `target` trong $i$ phần tử đầu tiên. Ban đầu, $f[0]=\infty$ và $ans=\infty$. Các chỉ số bắt đầu từ $1$.

Duyệt qua `arr`. Với vị trí hiện tại $i$, trước tiên đặt $f[i]=f[i-1]$ và cộng dồn prefix sum $s$. Nếu $s-\textit{target}$ tồn tại trong hash table, đặt $j=d[s-\textit{target}]$. Khi đó, đoạn $[j+1,i]$ có tổng bằng `target` và độ dài $i-j$. Cập nhật $f[i]=\min(f[i], i-j)$, đồng thời cập nhật đáp án bằng độ dài tốt nhất ở bên trái: $ans=\min(ans, f[j]+i-j)$. Sau đó lưu $d[s]=i$.

Cuối cùng, nếu $ans$ lớn hơn độ dài mảng thì trả về $-1$; ngược lại, trả về $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của `arr`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSumOfLengths(self, arr: List[int], target: int) -> int:
        d = {0: 0}
        s, n = 0, len(arr)
        f = [inf] * (n + 1)
        ans = inf
        for i, v in enumerate(arr, 1):
            s += v
            f[i] = f[i - 1]
            if s - target in d:
                j = d[s - target]
                f[i] = min(f[i], i - j)
                ans = min(ans, f[j] + i - j)
            d[s] = i
        return -1 if ans > n else ans
```

#### Java

```java
class Solution {
    public int minSumOfLengths(int[] arr, int target) {
        Map<Integer, Integer> d = new HashMap<>();
        d.put(0, 0);
        int n = arr.length;
        int[] f = new int[n + 1];
        final int inf = 1 << 30;
        f[0] = inf;
        int s = 0, ans = inf;
        for (int i = 1; i <= n; ++i) {
            int v = arr[i - 1];
            s += v;
            f[i] = f[i - 1];
            if (d.containsKey(s - target)) {
                int j = d.get(s - target);
                f[i] = Math.min(f[i], i - j);
                ans = Math.min(ans, f[j] + i - j);
            }
            d.put(s, i);
        }
        return ans > n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSumOfLengths(vector<int>& arr, int target) {
        unordered_map<int, int> d;
        d[0] = 0;
        int s = 0, n = arr.size();
        int f[n + 1];
        const int inf = 1 << 30;
        f[0] = inf;
        int ans = inf;
        for (int i = 1; i <= n; ++i) {
            int v = arr[i - 1];
            s += v;
            f[i] = f[i - 1];
            if (d.count(s - target)) {
                int j = d[s - target];
                f[i] = min(f[i], i - j);
                ans = min(ans, f[j] + i - j);
            }
            d[s] = i;
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minSumOfLengths(arr []int, target int) int {
	d := map[int]int{0: 0}
	const inf = 1 << 30
	s, n := 0, len(arr)
	f := make([]int, n+1)
	f[0] = inf
	ans := inf
	for i, v := range arr {
		i++
		f[i] = f[i-1]
		s += v
		if j, ok := d[s-target]; ok {
			f[i] = min(f[i], i-j)
			ans = min(ans, f[j]+i-j)
		}
		d[s] = i
	}
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minSumOfLengths(arr: number[], target: number): number {
    const d = new Map<number, number>();
    d.set(0, 0);
    let s = 0;
    const n = arr.length;
    const f: number[] = Array(n + 1);
    const inf = 1 << 30;
    f[0] = inf;
    let ans = inf;
    for (let i = 1; i <= n; ++i) {
        const v = arr[i - 1];
        s += v;
        f[i] = f[i - 1];
        if (d.has(s - target)) {
            const j = d.get(s - target)!;
            f[i] = Math.min(f[i], i - j);
            ans = Math.min(ans, f[j] + i - j);
        }
        d.set(s, i);
    }
    return ans > n ? -1 : ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_sum_of_lengths(arr: Vec<i32>, target: i32) -> i32 {
        let mut d = HashMap::new();
        d.insert(0, 0);
        let n = arr.len();
        let inf = 1 << 30;
        let mut f = vec![0; n + 1];
        f[0] = inf;
        let mut s = 0;
        let mut ans = inf;
        for i in 1..=n {
            s += arr[i - 1];
            f[i] = f[i - 1];
            if let Some(&j) = d.get(&(s - target)) {
                f[i] = f[i].min((i - j) as i32);
                ans = ans.min(f[j] + (i - j) as i32);
            }
            d.insert(s, i);
        }
        if ans > n as i32 {
            -1
        } else {
            ans
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
