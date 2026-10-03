---
comments: true
difficulty: Medium
tags:
    - Array
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [2198. Number of Single Divisor Triplets 🔒](https://leetcode.com/problems/number-of-single-divisor-triplets)

[Tài liệu tiếng Trung](/solution/2100-2199/2198.Number%20of%20Single%20Divisor%20Triplets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <strong>được đánh chỉ số từ 0</strong> <code>nums</code>. Một bộ ba gồm ba <strong>chỉ số phân biệt</strong> <code>(i, j, k)</code> được gọi là <strong>bộ ba có đúng một ước số</strong> của <code>nums</code> nếu <code>nums[i] + nums[j] + nums[k]</code> chia hết cho <strong>đúng một</strong> trong ba số <code>nums[i]</code>, <code>nums[j]</code> hoặc <code>nums[k]</code>.</p>
Trả về <em><strong>số lượng bộ ba có đúng một ước số</strong> của </em><code>nums</code><em>.</em>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,6,7,3,2]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:
</strong>Các bộ ba (0, 3, 4), (0, 4, 3), (3, 0, 4), (3, 4, 0), (4, 0, 3) và (4, 3, 0) có các giá trị [4, 3, 2] (hoặc một hoán vị của [4, 3, 2]).
4 + 3 + 2 = 9, chỉ chia hết cho 3, nên tất cả các bộ ba như vậy đều là bộ ba có đúng một ước số.
Các bộ ba (0, 2, 3), (0, 3, 2), (2, 0, 3), (2, 3, 0), (3, 0, 2) và (3, 2, 0) có các giá trị [4, 7, 3] (hoặc một hoán vị của [4, 7, 3]).
4 + 7 + 3 = 14, chỉ chia hết cho 7, nên tất cả các bộ ba như vậy đều là bộ ba có đúng một ước số.
Tổng cộng có 12 bộ ba có đúng một ước số.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Các bộ ba (0, 1, 2), (0, 2, 1), (1, 0, 2), (1, 2, 0), (2, 0, 1) và (2, 1, 0) có các giá trị [1, 2, 2] (hoặc một hoán vị của [1, 2, 2]).
1 + 2 + 2 = 5, chỉ chia hết cho 1, nên tất cả các bộ ba như vậy đều là bộ ba có đúng một ước số.
Tổng cộng có 6 bộ ba có đúng một ước số.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có bộ ba nào có đúng một ước số.
Lưu ý rằng (0, 1, 2) không phải là bộ ba có đúng một ước số vì nums[0] + nums[1] + nums[2] = 3 và 3 chia hết cho nums[0], nums[1] và nums[2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Tổng của một bộ ba phải chia hết cho đúng một trong ba giá trị. Các bộ ba chỉ số có thứ tự được tính, nhưng $n\le 10^5$ khiến việc liệt kê các chỉ số là không thể. Các giá trị nằm trong $[1,100]$, vì vậy ta liệt kê các giá trị rồi nhân với tần suất xuất hiện.
>
> Đếm từng giá trị, sau đó duyệt $a,b,c$ trong $[1,100]$. Nếu $a+b+c$ có đúng một ước số trong ba giá trị này, cộng tích các tần suất tương ứng, dùng $x(x-1)z$ khi có hai giá trị trùng nhau.
>
> Chi phí có bậc ba theo miền giá trị và không phụ thuộc vào $n$.

<!-- thinking:end -->

Ta nhận thấy miền giá trị của các phần tử trong mảng `nums` là $[1, 100]$. Vì vậy, ta có thể liệt kê ba số $a, b, c$, trong đó $a, b, c \in [1, 100]$, rồi xác định xem $a + b + c$ chỉ chia hết cho một trong ba số $a, b, c$ hay không. Nếu có, ta có thể tính số bộ ba có đúng một ước số với $a, b, c$ là các phần tử. Cách tính cụ thể như sau:

- Nếu $a = b$, số bộ ba có đúng một ước số với $a, b, c$ là các phần tử bằng $x \times (x - 1) \times z$, trong đó $x$, $y$, $z$ lần lượt là số lần xuất hiện của $a$, $b$, $c$ trong mảng `nums`.
- Nếu $a = c$, số bộ ba có đúng một ước số với $a, b, c$ là các phần tử bằng $x \times (x - 1) \times y$.
- Nếu $b = c$, số bộ ba có đúng một ước số với $a, b, c$ là các phần tử bằng $x \times y \times (y - 1)$.
- Nếu $a, b, c$ đôi một khác nhau, số bộ ba có đúng một ước số với $a, b, c$ là các phần tử bằng $x \times y \times z$.

Cuối cùng, ta cộng số lượng của tất cả các bộ ba có đúng một ước số.

Độ phức tạp thời gian là $O(M^3)$, độ phức tạp không gian là $O(M)$, trong đó $M$ là miền giá trị của các phần tử trong mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def singleDivisorTriplet(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        ans = 0
        for a, x in cnt.items():
            for b, y in cnt.items():
                for c, z in cnt.items():
                    s = a + b + c
                    if sum(s % v == 0 for v in (a, b, c)) == 1:
                        if a == b:
                            ans += x * (x - 1) * z
                        elif a == c:
                            ans += x * (x - 1) * y
                        elif b == c:
                            ans += x * y * (y - 1)
                        else:
                            ans += x * y * z
        return ans
```

#### Java

```java
class Solution {
    public long singleDivisorTriplet(int[] nums) {
        int[] cnt = new int[101];
        for (int x : nums) {
            ++cnt[x];
        }
        long ans = 0;
        for (int a = 1; a <= 100; ++a) {
            for (int b = 1; b <= 100; ++b) {
                for (int c = 1; c <= 100; ++c) {
                    int s = a + b + c;
                    int x = cnt[a], y = cnt[b], z = cnt[c];
                    int t = 0;
                    t += s % a == 0 ? 1 : 0;
                    t += s % b == 0 ? 1 : 0;
                    t += s % c == 0 ? 1 : 0;
                    if (t == 1) {
                        if (a == b) {
                            ans += 1L * x * (x - 1) * z;
                        } else if (a == c) {
                            ans += 1L * x * (x - 1) * y;
                        } else if (b == c) {
                            ans += 1L * x * y * (y - 1);
                        } else {
                            ans += 1L * x * y * z;
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
    long long singleDivisorTriplet(vector<int>& nums) {
        int cnt[101]{};
        for (int x : nums) {
            ++cnt[x];
        }
        long long ans = 0;
        for (int a = 1; a <= 100; ++a) {
            for (int b = 1; b <= 100; ++b) {
                for (int c = 1; c <= 100; ++c) {
                    int s = a + b + c;
                    int x = cnt[a], y = cnt[b], z = cnt[c];
                    int t = (s % a == 0) + (s % b == 0) + (s % c == 0);
                    if (t == 1) {
                        if (a == b) {
                            ans += 1LL * x * (x - 1) * z;
                        } else if (a == c) {
                            ans += 1LL * x * (x - 1) * y;
                        } else if (b == c) {
                            ans += 1LL * x * y * (y - 1);
                        } else {
                            ans += 1LL * x * y * z;
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func singleDivisorTriplet(nums []int) (ans int64) {
	cnt := [101]int{}
	for _, x := range nums {
		cnt[x]++
	}
	f := func(a, b int) int {
		if a%b == 0 {
			return 1
		}
		return 0
	}
	for a := 1; a <= 100; a++ {
		for b := 1; b <= 100; b++ {
			for c := 1; c <= 100; c++ {
				s := a + b + c
				t := f(s, a) + f(s, b) + f(s, c)
				if t == 1 {
					if a == b {
						ans += int64(cnt[a] * (cnt[a] - 1) * cnt[c])
					} else if a == c {
						ans += int64(cnt[a] * (cnt[a] - 1) * cnt[b])
					} else if b == c {
						ans += int64(cnt[b] * (cnt[b] - 1) * cnt[a])
					} else {
						ans += int64(cnt[a] * cnt[b] * cnt[c])
					}
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function singleDivisorTriplet(nums: number[]): number {
    const cnt: number[] = Array(101).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    let ans = 0;
    const f = (a: number, b: number) => (a % b === 0 ? 1 : 0);
    for (let a = 1; a <= 100; ++a) {
        for (let b = 1; b <= 100; ++b) {
            for (let c = 1; c <= 100; ++c) {
                const s = a + b + c;
                const t = f(s, a) + f(s, b) + f(s, c);
                if (t === 1) {
                    if (a === b) {
                        ans += cnt[a] * (cnt[a] - 1) * cnt[c];
                    } else if (a === c) {
                        ans += cnt[a] * (cnt[a] - 1) * cnt[b];
                    } else if (b === c) {
                        ans += cnt[b] * (cnt[b] - 1) * cnt[a];
                    } else {
                        ans += cnt[a] * cnt[b] * cnt[c];
                    }
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
