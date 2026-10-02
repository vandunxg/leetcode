---
comments: true
difficulty: Easy
rating: 1279
source: Weekly Contest 200 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [1534. Count Good Triplets](https://leetcode.com/problems/count-good-triplets)

[中文文档](/solution/1500-1599/1534.Count%20Good%20Triplets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> và ba số nguyên&nbsp;<code>a</code>,&nbsp;<code>b</code>&nbsp;và&nbsp;<code>c</code>. Hãy tìm số lượng bộ ba tốt.</p>

<p>Một bộ ba <code>(arr[i], arr[j], arr[k])</code>&nbsp;là <strong>tốt</strong> nếu thỏa các điều kiện sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; k &lt;&nbsp;arr.length</code></li>
	<li><code>|arr[i] - arr[j]| &lt;= a</code></li>
	<li><code>|arr[j] - arr[k]| &lt;= b</code></li>
	<li><code>|arr[i] - arr[k]| &lt;= c</code></li>
</ul>

<p>Trong đó <code>|x|</code> là giá trị tuyệt đối của <code>x</code>.</p>

<p>Trả về <em>số lượng bộ ba tốt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,0,1,1,9,7], a = 7, b = 2, c = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>&nbsp;Có 4 bộ ba tốt: [(3,0,1), (3,0,1), (3,1,1), (0,1,1)].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,1,2,2,3], a = 0, b = 0, c = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Không có bộ ba nào thỏa tất cả điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 100</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= a, b, c &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba chỉ số thỏa ba điều kiện giới hạn giá trị tuyệt đối. $n\le 100$, nên ba vòng lặp chỉ cần khoảng $10^6$ phép so sánh, phù hợp với giới hạn.
>
> Liệt kê $i<j<k$ và áp dụng trực tiếp ba bất đẳng thức. Các điều kiện độc lập, không có cấu trúc đơn điệu để biện minh cho một data structure phức tạp hơn.

<!-- thinking:end -->

Ta có thể liệt kê mọi $i$, $j$ và $k$ sao cho $i \lt j \lt k$, rồi kiểm tra đồng thời các điều kiện $|\textit{arr}[i] - \textit{arr}[j]| \le a$, $|\textit{arr}[j] - \textit{arr}[k]| \le b$ và $|\textit{arr}[i] - \textit{arr}[k]| \le c$. Nếu thỏa, tăng đáp án lên một.

Sau khi liệt kê mọi bộ ba có thể, ta thu được đáp án.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là độ dài mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodTriplets(self, arr: List[int], a: int, b: int, c: int) -> int:
        ans, n = 0, len(arr)
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    ans += (
                        abs(arr[i] - arr[j]) <= a
                        and abs(arr[j] - arr[k]) <= b
                        and abs(arr[i] - arr[k]) <= c
                    )
        return ans
```

#### Java

```java
class Solution {
    public int countGoodTriplets(int[] arr, int a, int b, int c) {
        int n = arr.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (Math.abs(arr[i] - arr[j]) <= a && Math.abs(arr[j] - arr[k]) <= b
                        && Math.abs(arr[i] - arr[k]) <= c) {
                        ++ans;
                    }
                }
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
    int countGoodTriplets(vector<int>& arr, int a, int b, int c) {
        int n = arr.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    ans += abs(arr[i] - arr[j]) <= a && abs(arr[j] - arr[k]) <= b && abs(arr[i] - arr[k]) <= c;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countGoodTriplets(arr []int, a int, b int, c int) (ans int) {
	n := len(arr)
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			for k := j + 1; k < n; k++ {
				if abs(arr[i]-arr[j]) <= a && abs(arr[j]-arr[k]) <= b && abs(arr[i]-arr[k]) <= c {
					ans++
				}
			}
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function countGoodTriplets(arr: number[], a: number, b: number, c: number): number {
    let n = arr.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; ++j) {
            for (let k = j + 1; k < n; ++k) {
                if (
                    Math.abs(arr[i] - arr[j]) <= a &&
                    Math.abs(arr[j] - arr[k]) <= b &&
                    Math.abs(arr[i] - arr[k]) <= c
                ) {
                    ++ans;
                }
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_good_triplets(arr: Vec<i32>, a: i32, b: i32, c: i32) -> i32 {
        let n = arr.len();
        let mut ans = 0;

        for i in 0..n {
            for j in i + 1..n {
                for k in j + 1..n {
                    if (arr[i] - arr[j]).abs() <= a && (arr[j] - arr[k]).abs() <= b && (arr[i] - arr[k]).abs() <= c {
                        ans += 1;
                    }
                }
            }
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int CountGoodTriplets(int[] arr, int a, int b, int c) {
        int n = arr.Length;
        int ans = 0;

        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (Math.Abs(arr[i] - arr[j]) <= a && Math.Abs(arr[j] - arr[k]) <= b && Math.Abs(arr[i] - arr[k]) <= c) {
                        ++ans;
                    }
                }
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
