---
comments: true
difficulty: Easy
rating: 1352
source: Weekly Contest 257 Q1
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [1995. Count Special Quadruplets](https://leetcode.com/problems/count-special-quadruplets)

[中文文档](/solution/1900-1999/1995.Count%20Special%20Quadruplets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong>, hãy trả về <em>số lượng <strong>bộ bốn phân biệt</strong></em> <code>(a, b, c, d)</code> <em>thỏa mãn:</em></p>

<ul>
	<li><code>nums[a] + nums[b] + nums[c] == nums[d]</code>, và</li>
	<li><code>a &lt; b &lt; c &lt; d</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,6]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bộ bốn duy nhất thỏa mãn yêu cầu là (0, 1, 2, 3) vì 1 + 2 + 3 == 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,6,4,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có bộ bốn nào như vậy trong [3,3,6,4,5].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,3,5]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 4 bộ bốn thỏa mãn yêu cầu là:
- (0, 1, 2, 3): 1 + 1 + 1 == 3
- (0, 1, 3, 4): 1 + 1 + 3 == 5
- (0, 2, 3, 4): 1 + 1 + 3 == 5
- (1, 2, 3, 4): 1 + 1 + 3 == 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>4 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 50$, bốn vòng lặp lồng nhau để duyệt $a<b<c<d$ và kiểm tra tổng là chấp nhận được.

<!-- thinking:end -->

Liệt kê $a < b < c < d$ bằng bốn vòng lặp lồng nhau.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for a in range(n - 3):
            for b in range(a + 1, n - 2):
                for c in range(b + 1, n - 1):
                    for d in range(c + 1, n):
                        if nums[a] + nums[b] + nums[c] == nums[d]:
                            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countQuadruplets(int[] nums) {
        int ans = 0, n = nums.length;
        for (int a = 0; a < n - 3; ++a) {
            for (int b = a + 1; b < n - 2; ++b) {
                for (int c = b + 1; c < n - 1; ++c) {
                    for (int d = c + 1; d < n; ++d) {
                        if (nums[a] + nums[b] + nums[c] == nums[d]) {
                            ++ans;
                        }
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
    int countQuadruplets(vector<int>& nums) {
        int ans = 0, n = nums.size();
        for (int a = 0; a < n - 3; ++a)
            for (int b = a + 1; b < n - 2; ++b)
                for (int c = b + 1; c < n - 1; ++c)
                    for (int d = c + 1; d < n; ++d)
                        if (nums[a] + nums[b] + nums[c] == nums[d]) ++ans;
        return ans;
    }
};
```

#### Go

```go
func countQuadruplets(nums []int) int {
	ans, n := 0, len(nums)
	for a := 0; a < n-3; a++ {
		for b := a + 1; b < n-2; b++ {
			for c := b + 1; c < n-1; c++ {
				for d := c + 1; d < n; d++ {
					if nums[a]+nums[b]+nums[c] == nums[d] {
						ans++
					}
				}
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Hệ số bậc bốn khá lớn. Cố định $c$ từ bên phải, đếm các giá trị của $d$ trong một map, rồi truy vấn $nums[a]+nums[b]+nums[c]$ với $a,b<c$, nhờ đó đạt $O(n^3)$.

<!-- thinking:end -->

Duyệt $c$ từ bên phải và đếm các giá trị của $d$, sau đó liệt kê $a,b$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        counter = Counter()
        for c in range(n - 2, 1, -1):
            counter[nums[c + 1]] += 1
            for a in range(c - 1):
                for b in range(a + 1, c):
                    ans += counter[nums[a] + nums[b] + nums[c]]
        return ans
```

#### Java

```java
class Solution {
    public int countQuadruplets(int[] nums) {
        int ans = 0, n = nums.length;
        int[] counter = new int[310];
        for (int c = n - 2; c > 1; --c) {
            ++counter[nums[c + 1]];
            for (int a = 0; a < c - 1; ++a) {
                for (int b = a + 1; b < c; ++b) {
                    ans += counter[nums[a] + nums[b] + nums[c]];
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
    int countQuadruplets(vector<int>& nums) {
        int ans = 0, n = nums.size();
        vector<int> counter(310);
        for (int c = n - 2; c > 1; --c) {
            ++counter[nums[c + 1]];
            for (int a = 0; a < c - 1; ++a) {
                for (int b = a + 1; b < c; ++b) {
                    ans += counter[nums[a] + nums[b] + nums[c]];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countQuadruplets(nums []int) int {
	ans, n := 0, len(nums)
	counter := make([]int, 310)
	for c := n - 2; c > 1; c-- {
		counter[nums[c+1]]++
		for a := 0; a < c-1; a++ {
			for b := a + 1; b < c; b++ {
				ans += counter[nums[a]+nums[b]+nums[c]]
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Bảng băm (Tối ưu)

<!-- thinking:start -->

> **Tư duy**
>
> Có thể giảm thêm một bước: duyệt $b$ từ bên phải, chèn mọi giá trị $d-c$ với $c=b+1$ vào một map, sau đó truy vấn $nums[a]+nums[b]$ với $a<b$, đạt $O(n^2)$.

<!-- thinking:end -->

Liệt kê $b$ đồng thời đếm $nums[d]-nums[c]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        counter = Counter()
        for b in range(n - 3, 0, -1):
            c = b + 1
            for d in range(c + 1, n):
                counter[nums[d] - nums[c]] += 1
            for a in range(b):
                ans += counter[nums[a] + nums[b]]
        return ans
```

#### Java

```java
class Solution {
    public int countQuadruplets(int[] nums) {
        int ans = 0, n = nums.length;
        int[] counter = new int[310];
        for (int b = n - 3; b > 0; --b) {
            int c = b + 1;
            for (int d = c + 1; d < n; ++d) {
                if (nums[d] - nums[c] >= 0) {
                    ++counter[nums[d] - nums[c]];
                }
            }
            for (int a = 0; a < b; ++a) {
                ans += counter[nums[a] + nums[b]];
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
    int countQuadruplets(vector<int>& nums) {
        int ans = 0, n = nums.size();
        vector<int> counter(310);
        for (int b = n - 3; b > 0; --b) {
            int c = b + 1;
            for (int d = c + 1; d < n; ++d) {
                if (nums[d] - nums[c] >= 0) {
                    ++counter[nums[d] - nums[c]];
                }
            }
            for (int a = 0; a < b; ++a) {
                ans += counter[nums[a] + nums[b]];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countQuadruplets(nums []int) int {
	ans, n := 0, len(nums)
	counter := make([]int, 310)
	for b := n - 3; b > 0; b-- {
		c := b + 1
		for d := c + 1; d < n; d++ {
			if nums[d] >= nums[c] {
				counter[nums[d]-nums[c]]++
			}
		}
		for a := 0; a < b; a++ {
			ans += counter[nums[a]+nums[b]]
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
