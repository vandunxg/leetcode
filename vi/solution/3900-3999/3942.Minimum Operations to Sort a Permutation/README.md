---
comments: true
difficulty: Medium
rating: 1854
source: Weekly Contest 503 Q3
tags:
    - Array
---

<!-- problem:start -->

# [3942. Minimum Operations to Sort a Permutation](https://leetcode.com/problems/minimum-operations-to-sort-a-permutation)

[中文文档](/solution/3900-3999/3942.Minimum%20Operations%20to%20Sort%20a%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, trong đó <code>nums</code> là một <span data-keyword="permutation-array">hoán vị</span> của các số nguyên từ 0 đến <code>n - 1</code>.</p>

<p>Bạn <strong>chỉ</strong> được phép thực hiện các thao tác sau:</p>

<ul>
	<li><strong>Đảo ngược</strong> toàn bộ mảng.</li>
	<li><strong>Xoay trái một vị trí</strong>: chuyển phần tử đầu tiên ra cuối mảng và dịch các phần tử còn lại sang trái một vị trí.</li>
</ul>

<p>Trả về một số nguyên biểu thị số thao tác <strong>ít nhất</strong> cần thực hiện để sắp xếp mảng theo thứ tự <strong>tăng dần</strong>. Nếu <strong>không thể</strong> sắp xếp mảng chỉ bằng các thao tác đã cho, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Xoay trái một vị trí: <code>[2, 1, 0]</code></li>
	<li>Đảo ngược mảng: <code>[0, 1, 2]</code></li>
</ul>

<p>Mảng được sắp xếp sau 2 thao tác, đây là số thao tác ít nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đảo ngược mảng: <code>[2, 0, 1]</code></li>
	<li>Xoay trái một vị trí: <code>[0, 1, 2]</code></li>
</ul>

<p>Mảng được sắp xếp sau 2 thao tác, đây là số thao tác ít nhất.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,0,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể biến <code>[2, 0, 1, 3]</code> thành mảng cần tìm. Do đó, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= n - 1</code></li>
	<li><code>nums</code> là một hoán vị của các số nguyên từ 0 đến <code>n - 1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Các thao tác được phép là các tổ hợp của xoay và đảo ngược, không phải các phép hoán vị tùy ý. Nếu có thể sắp xếp mảng, thứ tự vòng bắt đầu từ $0$ phải tăng dần theo một trong hai hướng.
>
> Xác định vị trí của $0$ tại $\textit{zero}$, rồi kiểm tra bước $+1$ và bước $-1$. Với mỗi hướng hợp lệ, số lần xoay và số lần thực hiện “đảo ngược–xoay–đảo ngược” được suy ra từ $\textit{zero}$ và $n$; lấy giá trị nhỏ nhất, hoặc báo không thể thực hiện nếu cả hai hướng đều không tạo ra mảng tăng dần.
>
> Việc kiểm tra có độ phức tạp $O(n)$ và không mô phỏng từng thao tác.

<!-- thinking:end -->

Trước hết, ta tìm vị trí của `0` trong mảng, gọi là $\textit{zero}$.

Tiếp theo, ta kiểm tra xem dãy có tăng dần khi duyệt sang phải từ `0` hay không, đồng thời kiểm tra điều tương tự khi duyệt sang trái từ `0`.

Nếu dãy tăng dần khi đi sang phải từ `0`, ta có thể sắp xếp mảng theo một trong hai cách:

- Xoay trực tiếp: xoay trái mảng $\textit{zero}$ vị trí.
- Đảo ngược, xoay rồi đảo ngược lại: đảo ngược mảng, xoay trái $n - \textit{zero}$ vị trí, sau đó đảo ngược mảng một lần nữa.

Nếu dãy tăng dần khi đi sang trái từ `0`, ta có thể sắp xếp mảng theo một trong hai cách:

- Xoay rồi đảo ngược: xoay trái $\textit{zero} + 1$ vị trí để đưa `0` ra cuối, sau đó đảo ngược mảng.
- Đảo ngược rồi xoay: đảo ngược mảng, sau đó xoay trái $n - \textit{zero} - 1$ vị trí để đưa `0` lên đầu.

Ta tính số thao tác của cả bốn cách trên và trả về giá trị nhỏ nhất. Nếu không thể sắp xếp, trả về `-1`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của $\textit{nums}$. Độ phức tạp bộ nhớ là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        n = len(nums)

        zero = nums.index(0)

        def check(step: int) -> bool:
            for i in range(1, n):
                prev = (zero + (i - 1) * step) % n
                curr = (zero + i * step) % n

                if nums[prev] > nums[curr]:
                    return False

            return True

        ans = inf

        if check(1):
            ans = min(ans, zero)
            ans = min(ans, n - zero + 2)

        if check(-1):
            ans = min(ans, zero + 2)
            ans = min(ans, n - zero)

        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int n = nums.length;

        int zero = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                zero = i;
                break;
            }
        }

        int finalZero = zero;

        IntPredicate check = step -> {
            for (int i = 1; i < n; i++) {
                int prev = (finalZero + (i - 1) * step + n) % n;
                int curr = (finalZero + i * step + n) % n;

                if (nums[prev] > nums[curr]) {
                    return false;
                }
            }

            return true;
        };

        int ans = Integer.MAX_VALUE;

        if (check.test(1)) {
            ans = Math.min(ans, zero);
            ans = Math.min(ans, n - zero + 2);
        }

        if (check.test(-1)) {
            ans = Math.min(ans, zero + 2);
            ans = Math.min(ans, n - zero);
        }

        return ans == Integer.MAX_VALUE ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int n = nums.size();

        int zero = ranges::find(nums, 0) - nums.begin();

        auto check = [&](int step) -> bool {
            for (int i = 1; i < n; i++) {
                int prev = (zero + (i - 1) * step + n) % n;
                int curr = (zero + i * step + n) % n;

                if (nums[prev] > nums[curr]) {
                    return false;
                }
            }
            return true;
        };

        int ans = INT_MAX;

        if (check(1)) {
            ans = min(ans, zero);
            ans = min(ans, n - zero + 2);
        }

        if (check(-1)) {
            ans = min(ans, zero + 2);
            ans = min(ans, n - zero);
        }

        return ans == INT_MAX ? -1 : ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	n := len(nums)

	zero := 0
	for i, x := range nums {
		if x == 0 {
			zero = i
			break
		}
	}

	check := func(step int) bool {
		for i := 1; i < n; i++ {
			prev := (zero + (i-1)*step + n) % n
			curr := (zero + i*step + n) % n

			if nums[prev] > nums[curr] {
				return false
			}
		}

		return true
	}

	ans := math.MaxInt

	if check(1) {
		ans = min(ans, zero)
		ans = min(ans, n-zero+2)
	}

	if check(-1) {
		ans = min(ans, zero+2)
		ans = min(ans, n-zero)
	}

	if ans == math.MaxInt {
		return -1
	}

	return ans
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const n = nums.length;

    const zero = nums.indexOf(0);

    const check = (step: number): boolean => {
        for (let i = 1; i < n; i++) {
            const prev = (zero + (i - 1) * step + n) % n;
            const curr = (zero + i * step + n) % n;

            if (nums[prev] > nums[curr]) {
                return false;
            }
        }

        return true;
    };

    let ans = Number.MAX_SAFE_INTEGER;

    if (check(1)) {
        ans = Math.min(ans, zero);
        ans = Math.min(ans, n - zero + 2);
    }

    if (check(-1)) {
        ans = Math.min(ans, zero + 2);
        ans = Math.min(ans, n - zero);
    }

    return ans === Number.MAX_SAFE_INTEGER ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
