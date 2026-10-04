---
comments: true
difficulty: Medium
rating: 1502
source: Biweekly Contest 105 Q3
tags:
    - Greedy
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [2708. Maximum Strength of a Group](https://leetcode.com/problems/maximum-strength-of-a-group)

[中文文档](/solution/2700-2799/2708.Maximum%20Strength%20of%20a%20Group/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn điểm số của học sinh trong một kỳ thi. Giáo viên muốn lập một nhóm <strong>không rỗng</strong> gồm các học sinh có <strong>strength</strong> lớn nhất, trong đó strength của một nhóm gồm các học sinh có chỉ số <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, <code>i<sub>2</sub></code>, ... , <code>i<sub>k</sub></code> được định nghĩa là <code>nums[i<sub>0</sub>] * nums[i<sub>1</sub>] * nums[i<sub>2</sub>] * ... * nums[i<sub>k</sub>​]</code>.</p>

<p>Hãy trả về <em>strength lớn nhất của một nhóm mà giáo viên có thể lập được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,-1,-5,2,5,-9]
<strong>Output:</strong> 1350
<strong>Giải thích:</strong> Một cách để lập nhóm có strength lớn nhất là chọn các học sinh tại các chỉ số [0,2,3,4,5]. Strength của nhóm là 3 * (-5) * 2 * 5 * (-9) = 1350, và có thể chứng minh đây là giá trị tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [-4,-5,-4]
<strong>Output:</strong> 20
<strong>Giải thích:</strong> Chọn các học sinh tại các chỉ số [0, 1]. Khi đó, strength thu được là 20. Không thể đạt được giá trị lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 13</code></li>
	<li><code>-9 &lt;= nums[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Strength là tích của một tập con không rỗng, và ta cần tìm giá trị lớn nhất. Vì $n\le 13$ nên có $2^n-1$ tập con, do đó ta có thể tính tích của từng mask.
>
> Bỏ qua tập rỗng; các số âm và số 0 đều tham gia vào tích, nên không cần chia trường hợp trước.

<!-- thinking:end -->

Bài toán thực chất là tìm tích lớn nhất trong tất cả các tập con. Vì độ dài mảng không vượt quá $13$, ta có thể sử dụng phương pháp liệt kê nhị phân.

Ta liệt kê tất cả các tập con trong khoảng $[1, 2^n)$, tính tích của từng tập con rồi trả về giá trị lớn nhất.

Độ phức tạp thời gian là $O(2^n \times n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxStrength(self, nums: List[int]) -> int:
        ans = -inf
        for i in range(1, 1 << len(nums)):
            t = 1
            for j, x in enumerate(nums):
                if i >> j & 1:
                    t *= x
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public long maxStrength(int[] nums) {
        long ans = (long) -1e14;
        int n = nums.length;
        for (int i = 1; i < 1 << n; ++i) {
            long t = 1;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    t *= nums[j];
                }
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxStrength(vector<int>& nums) {
        long long ans = -1e14;
        int n = nums.size();
        for (int i = 1; i < 1 << n; ++i) {
            long long t = 1;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    t *= nums[j];
                }
            }
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maxStrength(nums []int) int64 {
	ans := int64(-1e14)
	for i := 1; i < 1<<len(nums); i++ {
		var t int64 = 1
		for j, x := range nums {
			if i>>j&1 == 1 {
				t *= int64(x)
			}
		}
		ans = max(ans, t)
	}
	return ans
}
```

#### TypeScript

```ts
function maxStrength(nums: number[]): number {
    let ans = -Infinity;
    const n = nums.length;
    for (let i = 1; i < 1 << n; ++i) {
        let t = 1;
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                t *= nums[j];
            }
        }
        ans = Math.max(ans, t);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê nhị phân có độ phức tạp theo cấp số mũ đối với $n$. Sau khi phân nhóm theo dấu, ta nên tránh số 0, còn các số âm chỉ có ích khi được ghép thành từng cặp. Hãy sắp xếp mảng rồi duyệt: nhân hai số âm liên tiếp với nhau, bỏ qua số âm lẻ hoặc số 0, và lấy mọi số dương. Mảng chỉ có một phần tử và mảng toàn số 0 được xử lý riêng.

<!-- thinking:end -->

Trước hết, ta có thể sắp xếp mảng. Dựa trên các đặc điểm của mảng, ta có những kết luận sau:

- Nếu mảng chỉ có một phần tử, giá trị strength lớn nhất chính là phần tử đó.
- Nếu mảng có từ hai phần tử trở lên và $nums[1] = nums[n - 1] = 0$, giá trị strength lớn nhất là $0$.
- Nếu không, ta duyệt mảng từ nhỏ đến lớn. Nếu phần tử hiện tại nhỏ hơn $0$ và phần tử tiếp theo cũng nhỏ hơn $0$, ta nhân hai phần tử này rồi tích lũy vào đáp án. Ngược lại, nếu phần tử hiện tại nhỏ hơn hoặc bằng $0$, ta bỏ qua ngay. Nếu phần tử hiện tại lớn hơn $0$, ta nhân phần tử này vào đáp án. Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxStrength(self, nums: List[int]) -> int:
        nums.sort()
        n = len(nums)
        if n == 1:
            return nums[0]
        if nums[1] == nums[-1] == 0:
            return 0
        ans, i = 1, 0
        while i < n:
            if nums[i] < 0 and i + 1 < n and nums[i + 1] < 0:
                ans *= nums[i] * nums[i + 1]
                i += 2
            elif nums[i] <= 0:
                i += 1
            else:
                ans *= nums[i]
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public long maxStrength(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        if (n == 1) {
            return nums[0];
        }
        if (nums[1] == 0 && nums[n - 1] == 0) {
            return 0;
        }
        long ans = 1;
        int i = 0;
        while (i < n) {
            if (nums[i] < 0 && i + 1 < n && nums[i + 1] < 0) {
                ans *= nums[i] * nums[i + 1];
                i += 2;
            } else if (nums[i] <= 0) {
                i += 1;
            } else {
                ans *= nums[i];
                i += 1;
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
    long long maxStrength(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        if (n == 1) {
            return nums[0];
        }
        if (nums[1] == 0 && nums[n - 1] == 0) {
            return 0;
        }
        long long ans = 1;
        int i = 0;
        while (i < n) {
            if (nums[i] < 0 && i + 1 < n && nums[i + 1] < 0) {
                ans *= nums[i] * nums[i + 1];
                i += 2;
            } else if (nums[i] <= 0) {
                i += 1;
            } else {
                ans *= nums[i];
                i += 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxStrength(nums []int) int64 {
	sort.Ints(nums)
	n := len(nums)
	if n == 1 {
		return int64(nums[0])
	}
	if nums[1] == 0 && nums[n-1] == 0 {
		return 0
	}
	ans := int64(1)
	for i := 0; i < n; i++ {
		if nums[i] < 0 && i+1 < n && nums[i+1] < 0 {
			ans *= int64(nums[i] * nums[i+1])
			i++
		} else if nums[i] > 0 {
			ans *= int64(nums[i])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxStrength(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    if (n === 1) {
        return nums[0];
    }
    if (nums[1] === 0 && nums[n - 1] === 0) {
        return 0;
    }
    let ans = 1;
    for (let i = 0; i < n; ++i) {
        if (nums[i] < 0 && i + 1 < n && nums[i + 1] < 0) {
            ans *= nums[i] * nums[i + 1];
            ++i;
        } else if (nums[i] > 0) {
            ans *= nums[i];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
