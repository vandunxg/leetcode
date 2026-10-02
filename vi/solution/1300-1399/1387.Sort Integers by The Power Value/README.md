---
comments: true
difficulty: Medium
rating: 1506
source: Biweekly Contest 22 Q3
tags:
    - Memoization
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1387. Sort Integers by The Power Value](https://leetcode.com/problems/sort-integers-by-the-power-value)

[中文文档](/solution/1300-1399/1387.Sort%20Integers%20by%20The%20Power%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Power của số nguyên <code>x</code> được định nghĩa là số bước cần thiết để biến đổi <code>x</code> thành <code>1</code> theo các quy tắc sau:</p>

<ul>
	<li>Nếu <code>x</code> chẵn thì <code>x = x / 2</code></li>
	<li>Nếu <code>x</code> lẻ thì <code>x = 3 * x + 1</code></li>
</ul>

<p>Ví dụ, power của <code>x = 3</code> là <code>7</code> vì cần <code>7</code> bước để biến <code>3</code> thành <code>1</code> (<code>3 --&gt; 10 --&gt; 5 --&gt; 16 --&gt; 8 --&gt; 4 --&gt; 2 --&gt; 1</code>).</p>

<p>Cho ba số nguyên <code>lo</code>, <code>hi</code> và <code>k</code>. Hãy sắp xếp các số nguyên trong đoạn <code>[lo, hi]</code> theo giá trị power <strong>tăng dần</strong>. Nếu hai hay nhiều số có cùng giá trị power, hãy sắp xếp chúng theo giá trị số <strong>tăng dần</strong>.</p>

<p>Trả về số nguyên đứng thứ <code>k<sup>th</sup></code> trong đoạn <code>[lo, hi]</code> sau khi sắp xếp theo giá trị power.</p>

<p>Lưu ý, với mọi số nguyên <code>x</code> thỏa mãn <code>(lo &lt;= x &lt;= hi)</code>, đề bài <strong>đảm bảo</strong> rằng <code>x</code> sẽ biến đổi thành <code>1</code> theo các quy tắc trên và giá trị power của <code>x</code> sẽ <strong>nằm trong</strong> phạm vi số nguyên có dấu 32 bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> lo = 12, hi = 15, k = 2
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Power của 12 là 9 (12 --&gt; 6 --&gt; 3 --&gt; 10 --&gt; 5 --&gt; 16 --&gt; 8 --&gt; 4 --&gt; 2 --&gt; 1)
Power của 13 là 9
Power của 14 là 17
Power của 15 là 17
Sau khi sắp xếp đoạn theo giá trị power, ta được [12,13,14,15]. Với k = 2, đáp án là phần tử thứ hai, tức 13.
Lưu ý 12 và 13 có cùng giá trị power nên được sắp xếp theo thứ tự tăng dần; 14 và 15 cũng vậy.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> lo = 7, hi = 11, k = 4
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Mảng giá trị power tương ứng với đoạn [7, 8, 9, 10, 11] là [16, 3, 19, 6, 14].
Đoạn sau khi sắp xếp theo power là [8, 10, 11, 7, 9].
Số đứng thứ tư trong mảng đã sắp xếp là 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= lo &lt;= hi &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= hi - lo + 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp $[lo,hi]$ theo số bước Collatz để đến $1$, rồi theo giá trị số, và trả về phần tử thứ $k$. Đoạn có tối đa $1000$ số nguyên nên chỉ cần mô phỏng quá trình với từng $x$; cache giúp tránh tính lại cùng một chuỗi. Sắp xếp theo $f$ rồi lấy phần tử ở chỉ số $k-1$ là có đáp án.

<!-- thinking:end -->

Trước tiên, ta định nghĩa hàm $\textit{f}(x)$ biểu diễn số bước cần để biến đổi số $x$ thành $1$, tức giá trị power của $x$.

Sau đó, ta sắp xếp các số trong đoạn $[\textit{lo}, \textit{hi}]$ theo giá trị power tăng dần. Nếu các giá trị power bằng nhau, ta sắp xếp theo chính giá trị số tăng dần.

Cuối cùng, ta trả về số đứng thứ $k$ trong danh sách đã sắp xếp.

Độ phức tạp thời gian là $O(n \times \log n \times M)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng số trong đoạn $[\textit{lo}, \textit{hi}]$, còn $M$ là giá trị lớn nhất của $f(x)$, tối đa bằng $178$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
@cache
def f(x: int) -> int:
    ans = 0
    while x != 1:
        if x % 2 == 0:
            x //= 2
        else:
            x = 3 * x + 1
        ans += 1
    return ans


class Solution:
    def getKth(self, lo: int, hi: int, k: int) -> int:
        return sorted(range(lo, hi + 1), key=f)[k - 1]
```

#### Java

```java
class Solution {
    public int getKth(int lo, int hi, int k) {
        Integer[] nums = new Integer[hi - lo + 1];
        for (int i = lo; i <= hi; ++i) {
            nums[i - lo] = i;
        }
        Arrays.sort(nums, (a, b) -> {
            int fa = f(a), fb = f(b);
            return fa == fb ? a - b : fa - fb;
        });
        return nums[k - 1];
    }

    private int f(int x) {
        int ans = 0;
        for (; x != 1; ++ans) {
            if (x % 2 == 0) {
                x /= 2;
            } else {
                x = x * 3 + 1;
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
    int getKth(int lo, int hi, int k) {
        auto f = [](int x) {
            int ans = 0;
            for (; x != 1; ++ans) {
                if (x % 2 == 0) {
                    x /= 2;
                } else {
                    x = 3 * x + 1;
                }
            }
            return ans;
        };
        vector<int> nums;
        for (int i = lo; i <= hi; ++i) {
            nums.push_back(i);
        }
        sort(nums.begin(), nums.end(), [&](int x, int y) {
            int fx = f(x), fy = f(y);
            if (fx != fy) {
                return fx < fy;
            } else {
                return x < y;
            }
        });
        return nums[k - 1];
    }
};
```

#### Go

```go
func getKth(lo int, hi int, k int) int {
	f := func(x int) (ans int) {
		for ; x != 1; ans++ {
			if x%2 == 0 {
				x /= 2
			} else {
				x = 3*x + 1
			}
		}
		return
	}
	nums := make([]int, hi-lo+1)
	for i := range nums {
		nums[i] = lo + i
	}
	sort.Slice(nums, func(i, j int) bool {
		fx, fy := f(nums[i]), f(nums[j])
		if fx != fy {
			return fx < fy
		}
		return nums[i] < nums[j]
	})
	return nums[k-1]
}
```

#### TypeScript

```ts
function getKth(lo: number, hi: number, k: number): number {
    const f = (x: number): number => {
        let ans = 0;
        for (; x !== 1; ++ans) {
            if (x % 2 === 0) {
                x >>= 1;
            } else {
                x = x * 3 + 1;
            }
        }
        return ans;
    };
    const nums = new Array(hi - lo + 1).fill(0).map((_, i) => i + lo);
    nums.sort((a, b) => {
        const fa = f(a),
            fb = f(b);
        return fa === fb ? a - b : fa - fb;
    });
    return nums[k - 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn get_kth(lo: i32, hi: i32, k: i32) -> i32 {
        let f = |mut x: i32| -> i32 {
            let mut ans = 0;
            while x != 1 {
                if x % 2 == 0 {
                    x /= 2;
                } else {
                    x = 3 * x + 1;
                }
                ans += 1;
            }
            ans
        };

        let mut nums: Vec<i32> = (lo..=hi).collect();
        nums.sort_by(|&x, &y| f(x).cmp(&f(y)));
        nums[(k - 1) as usize]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
