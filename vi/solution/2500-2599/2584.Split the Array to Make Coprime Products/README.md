---
comments: true
difficulty: Hard
rating: 2159
source: Weekly Contest 335 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Number Theory
    - Prime Factorization
---

<!-- problem:start -->

# [2584. Split the Array to Make Coprime Products](https://leetcode.com/problems/split-the-array-to-make-coprime-products)

[中文文档](/solution/2500-2599/2584.Split%20the%20Array%20to%20Make%20Coprime%20Products/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>.</p>

<p>Một <strong>phép chia</strong> tại chỉ số <code>i</code>, với <code>0 &lt;= i &lt;= n - 2</code>, được gọi là <strong>hợp lệ</strong> nếu tích của <code>i + 1</code> phần tử đầu tiên và tích của các phần tử còn lại là nguyên tố cùng nhau.</p>

<ul>
	<li>Ví dụ, nếu <code>nums = [2, 3, 3]</code>, phép chia tại chỉ số <code>i = 0</code> là hợp lệ vì <code>2</code> và <code>9</code> là nguyên tố cùng nhau, còn phép chia tại chỉ số <code>i = 1</code> không hợp lệ vì <code>6</code> và <code>3</code> không nguyên tố cùng nhau. Phép chia tại chỉ số <code>i = 2</code> không hợp lệ vì <code>i == n - 1</code>.</li>
</ul>

<p>Trả về <em>chỉ số nhỏ nhất </em><code>i</code><em> mà tại đó mảng có thể được chia hợp lệ, hoặc </em><code>-1</code><em> nếu không tồn tại phép chia như vậy</em>.</p>

<p>Hai giá trị <code>val1</code> và <code>val2</code> là nguyên tố cùng nhau nếu <code>gcd(val1, val2) == 1</code>, trong đó <code>gcd(val1, val2)</code> là ước chung lớn nhất của <code>val1</code> và <code>val2</code>.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2584.Split%20the%20Array%20to%20Make%20Coprime%20Products/images/second.png" style="width: 450px; height: 211px;" />
<pre>
<strong>Đầu vào:</strong> nums = [4,7,8,15,3,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bảng trên cho biết giá trị của tích i + 1 phần tử đầu tiên, các phần tử còn lại và gcd của chúng tại mỗi chỉ số i.
Phép chia hợp lệ duy nhất là tại chỉ số 2.
</pre>

<p><strong>Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2500-2599/2584.Split%20the%20Array%20to%20Make%20Coprime%20Products/images/capture.png" style="width: 450px; height: 215px;" />
<pre>
<strong>Đầu vào:</strong> nums = [4,7,15,8,3,5]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Bảng trên cho biết giá trị của tích i + 1 phần tử đầu tiên, các phần tử còn lại và gcd của chúng tại mỗi chỉ số i.
Không có phép chia hợp lệ nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm chỉ số $i$ nhỏ nhất sao cho tích prefix nguyên tố cùng nhau với tích suffix. Các tích có thể bị tràn; hai tích nguyên tố cùng nhau có nghĩa là chúng không có thừa số nguyên tố chung.
>
> Lần xuất hiện đầu tiên và cuối cùng của mỗi số nguyên tố phải nằm cùng một phía của điểm chia. Lưu chỉ số đầu tiên của từng số nguyên tố và mở rộng phạm vi bắt đầu tại chỉ số đó đến lần xuất hiện cuối cùng. Duyệt các phạm vi này: nếu đầu mút phải đang xét kết thúc trước chỉ số $i$, thì $i-1$ là một phép chia hợp lệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findValidSplit(self, nums: List[int]) -> int:
        first = {}
        n = len(nums)
        last = list(range(n))
        for i, x in enumerate(nums):
            j = 2
            while j <= x // j:
                if x % j == 0:
                    if j in first:
                        last[first[j]] = i
                    else:
                        first[j] = i
                    while x % j == 0:
                        x //= j
                j += 1
            if x > 1:
                if x in first:
                    last[first[x]] = i
                else:
                    first[x] = i
        mx = last[0]
        for i, x in enumerate(last):
            if mx < i:
                return mx
            mx = max(mx, x)
        return -1
```

#### Java

```java
class Solution {
    public int findValidSplit(int[] nums) {
        Map<Integer, Integer> first = new HashMap<>();
        int n = nums.length;
        int[] last = new int[n];
        for (int i = 0; i < n; ++i) {
            last[i] = i;
        }
        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            for (int j = 2; j <= x / j; ++j) {
                if (x % j == 0) {
                    if (first.containsKey(j)) {
                        last[first.get(j)] = i;
                    } else {
                        first.put(j, i);
                    }
                    while (x % j == 0) {
                        x /= j;
                    }
                }
            }
            if (x > 1) {
                if (first.containsKey(x)) {
                    last[first.get(x)] = i;
                } else {
                    first.put(x, i);
                }
            }
        }
        int mx = last[0];
        for (int i = 0; i < n; ++i) {
            if (mx < i) {
                return mx;
            }
            mx = Math.max(mx, last[i]);
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findValidSplit(vector<int>& nums) {
        unordered_map<int, int> first;
        int n = nums.size();
        vector<int> last(n);
        iota(last.begin(), last.end(), 0);
        for (int i = 0; i < n; ++i) {
            int x = nums[i];
            for (int j = 2; j <= x / j; ++j) {
                if (x % j == 0) {
                    if (first.count(j)) {
                        last[first[j]] = i;
                    } else {
                        first[j] = i;
                    }
                    while (x % j == 0) {
                        x /= j;
                    }
                }
            }
            if (x > 1) {
                if (first.count(x)) {
                    last[first[x]] = i;
                } else {
                    first[x] = i;
                }
            }
        }
        int mx = last[0];
        for (int i = 0; i < n; ++i) {
            if (mx < i) {
                return mx;
            }
            mx = max(mx, last[i]);
        }
        return -1;
    }
};
```

#### Go

```go
func findValidSplit(nums []int) int {
	first := map[int]int{}
	n := len(nums)
	last := make([]int, n)
	for i := range last {
		last[i] = i
	}
	for i, x := range nums {
		for j := 2; j <= x/j; j++ {
			if x%j == 0 {
				if k, ok := first[j]; ok {
					last[k] = i
				} else {
					first[j] = i
				}
				for x%j == 0 {
					x /= j
				}
			}
		}
		if x > 1 {
			if k, ok := first[x]; ok {
				last[k] = i
			} else {
				first[x] = i
			}
		}
	}
	mx := last[0]
	for i, x := range last {
		if mx < i {
			return mx
		}
		mx = max(mx, x)
	}
	return -1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
