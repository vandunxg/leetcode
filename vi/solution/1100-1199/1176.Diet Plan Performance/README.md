---
comments: true
difficulty: Easy
rating: 1397
source: Weekly Contest 152 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [1176. Diet Plan Performance 🔒](https://leetcode.com/problems/diet-plan-performance)

[中文文档](/solution/1100-1199/1176.Diet%20Plan%20Performance/README.md)

## Mô tả

<!-- description:start -->

<p>Một người đang ăn kiêng tiêu thụ&nbsp;<code>calories[i]</code>&nbsp;calo vào ngày thứ <code>i</code>.&nbsp;</p>

<p>Cho số nguyên <code>k</code>. Với <strong>mọi</strong> dãy gồm <code>k</code> ngày liên tiếp (<code>calories[i], calories[i+1], ..., calories[i+k-1]</code>&nbsp;với mọi <code>0 &lt;= i &lt;= n-k</code>), người đó tính <em>T</em>, tổng lượng calo tiêu thụ trong dãy <code>k</code> ngày đó (<code>calories[i] + calories[i+1] + ... + calories[i+k-1]</code>):</p>

<ul>
	<li>Nếu <code>T &lt; lower</code>, họ ăn kiêng không tốt và bị trừ 1 điểm;&nbsp;</li>
	<li>Nếu <code>T &gt; upper</code>, họ ăn kiêng tốt và được cộng 1 điểm;</li>
	<li>Nếu không, họ ăn kiêng bình thường và điểm số không thay đổi.</li>
</ul>

<p>Ban đầu, người đó có 0 điểm. Hãy trả về tổng điểm sau khi ăn kiêng trong <code>calories.length</code>&nbsp;ngày.</p>

<p>Lưu ý tổng điểm có thể âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> calories = [1,2,3,4,5], k = 1, lower = 3, upper = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích</strong>: Vì k = 1, ta xét riêng từng phần tử của mảng và so sánh với lower và upper.
calories[0] và calories[1] nhỏ hơn lower nên bị trừ 2 điểm.
calories[3] và calories[4] lớn hơn upper nên được cộng 2 điểm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> calories = [3,2], k = 2, lower = 0, upper = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>: Vì k = 2, ta xét các mảng con có độ dài 2.
calories[0] + calories[1] &gt; upper nên được cộng 1 điểm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> calories = [6,5,0,0], k = 2, lower = 1, upper = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích</strong>:
calories[0] + calories[1] &gt; upper nên được cộng 1 điểm.
lower &lt;= calories[1] + calories[2] &lt;= upper nên điểm số không thay đổi.
calories[2] + calories[3] &lt; lower nên bị trừ 1 điểm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= calories.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= calories[i] &lt;= 20000</code></li>
	<li><code>0 &lt;= lower &lt;= upper</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Tổng calo của mỗi cửa sổ dài $k$ được so sánh với hai ngưỡng. Tổng tiền tố giúp tính $s[i+k]-s[i]$ trong $O(1)$, vì vậy ta chỉ cần duyệt các vị trí bắt đầu cửa sổ.

<!-- thinking:end -->

Trước tiên, tính trước mảng tổng tiền tố $s$ có độ dài $n+1$, trong đó $s[i]$ là tổng calo của $i$ ngày đầu tiên.

Sau đó, duyệt mảng tổng tiền tố $s$. Với mỗi vị trí $i$, tính $s[i+k]-s[i]$, tức tổng calo trong $k$ ngày liên tiếp bắt đầu từ ngày thứ $i$. Theo đề bài, ta so sánh từng giá trị $s[i+k]-s[i]$ với $lower$ và $upper$, rồi cập nhật đáp án tương ứng.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài mảng `calories`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dietPlanPerformance(
        self, calories: List[int], k: int, lower: int, upper: int
    ) -> int:
        s = list(accumulate(calories, initial=0))
        ans, n = 0, len(calories)
        for i in range(n - k + 1):
            t = s[i + k] - s[i]
            if t < lower:
                ans -= 1
            elif t > upper:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int dietPlanPerformance(int[] calories, int k, int lower, int upper) {
        int n = calories.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + calories[i];
        }
        int ans = 0;
        for (int i = 0; i < n - k + 1; ++i) {
            int t = s[i + k] - s[i];
            if (t < lower) {
                --ans;
            } else if (t > upper) {
                ++ans;
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
    int dietPlanPerformance(vector<int>& calories, int k, int lower, int upper) {
        int n = calories.size();
        int s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + calories[i];
        }
        int ans = 0;
        for (int i = 0; i < n - k + 1; ++i) {
            int t = s[i + k] - s[i];
            if (t < lower) {
                --ans;
            } else if (t > upper) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func dietPlanPerformance(calories []int, k int, lower int, upper int) (ans int) {
	n := len(calories)
	s := make([]int, n+1)
	for i, x := range calories {
		s[i+1] = s[i] + x
	}
	for i := 0; i < n-k+1; i++ {
		t := s[i+k] - s[i]
		if t < lower {
			ans--
		} else if t > upper {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function dietPlanPerformance(calories: number[], k: number, lower: number, upper: number): number {
    const n = calories.length;
    const s: number[] = new Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + calories[i];
    }
    let ans = 0;
    for (let i = 0; i < n - k + 1; ++i) {
        const t = s[i + k] - s[i];
        if (t < lower) {
            --ans;
        } else if (t > upper) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 lưu mảng tổng tiền tố tốn $O(n)$ bộ nhớ. Với cửa sổ cố định, chỉ cần duy trì tổng hiện tại: cộng ngày mới vào và trừ ngày vừa rời cửa sổ. Nhờ đó, bộ nhớ giảm còn hằng số; quy tắc tính điểm vẫn giữ nguyên.

<!-- thinking:end -->

Ta duy trì cửa sổ trượt có độ dài $k$, gọi tổng các phần tử trong cửa sổ là $s$. Nếu $s \lt lower$, điểm giảm $1$; nếu $s > upper$, điểm tăng $1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng `calories`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dietPlanPerformance(
        self, calories: List[int], k: int, lower: int, upper: int
    ) -> int:
        def check(s):
            if s < lower:
                return -1
            if s > upper:
                return 1
            return 0

        s, n = sum(calories[:k]), len(calories)
        ans = check(s)
        for i in range(k, n):
            s += calories[i] - calories[i - k]
            ans += check(s)
        return ans
```

#### Java

```java
class Solution {
    public int dietPlanPerformance(int[] calories, int k, int lower, int upper) {
        int s = 0, n = calories.length;
        for (int i = 0; i < k; ++i) {
            s += calories[i];
        }
        int ans = 0;
        if (s < lower) {
            --ans;
        } else if (s > upper) {
            ++ans;
        }
        for (int i = k; i < n; ++i) {
            s += calories[i] - calories[i - k];
            if (s < lower) {
                --ans;
            } else if (s > upper) {
                ++ans;
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
    int dietPlanPerformance(vector<int>& calories, int k, int lower, int upper) {
        int n = calories.size();
        int s = accumulate(calories.begin(), calories.begin() + k, 0);
        int ans = 0;
        if (s < lower) {
            --ans;
        } else if (s > upper) {
            ++ans;
        }
        for (int i = k; i < n; ++i) {
            s += calories[i] - calories[i - k];
            if (s < lower) {
                --ans;
            } else if (s > upper) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func dietPlanPerformance(calories []int, k int, lower int, upper int) (ans int) {
	n := len(calories)
	s := 0
	for _, x := range calories[:k] {
		s += x
	}
	if s < lower {
		ans--
	} else if s > upper {
		ans++
	}
	for i := k; i < n; i++ {
		s += calories[i] - calories[i-k]
		if s < lower {
			ans--
		} else if s > upper {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function dietPlanPerformance(calories: number[], k: number, lower: number, upper: number): number {
    const n = calories.length;
    let s = calories.slice(0, k).reduce((a, b) => a + b);
    let ans = 0;
    if (s < lower) {
        --ans;
    } else if (s > upper) {
        ++ans;
    }
    for (let i = k; i < n; ++i) {
        s += calories[i] - calories[i - k];
        if (s < lower) {
            --ans;
        } else if (s > upper) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
