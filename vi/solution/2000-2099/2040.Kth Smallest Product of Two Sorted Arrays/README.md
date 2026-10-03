---
comments: true
difficulty: Hard
rating: 2517
source: Biweekly Contest 63 Q4
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2040. Kth Smallest Product of Two Sorted Arrays](https://leetcode.com/problems/kth-smallest-product-of-two-sorted-arrays)

[中文文档](/solution/2000-2099/2040.Kth%20Smallest%20Product%20of%20Two%20Sorted%20Arrays/README.md)

## Mô tả

<!-- description:start -->

Cho hai mảng số nguyên <strong>đã sắp xếp, đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code>, cùng với một số nguyên <code>k</code>. Hãy trả về <em>tích thứ </em><code>k<sup>th</sup></code><em> (<strong>đánh số từ 1</strong>) nhỏ nhất trong các tích </em><code>nums1[i] * nums2[j]</code><em> với </em><code>0 &lt;= i &lt; nums1.length</code><em> và </em><code>0 &lt;= j &lt; nums2.length</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,5], nums2 = [3,4], k = 2
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Hai tích nhỏ nhất là:
- nums1[0] * nums2[0] = 2 * 3 = 6
- nums1[0] * nums2[1] = 2 * 4 = 8
Tích nhỏ thứ 2<sup>nd</sup> là 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [-4,-2,0,3], nums2 = [2,4], k = 6
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Sáu tích nhỏ nhất là:
- nums1[0] * nums2[1] = (-4) * 4 = -16
- nums1[0] * nums2[0] = (-4) * 2 = -8
- nums1[1] * nums2[1] = (-2) * 4 = -8
- nums1[1] * nums2[0] = (-2) * 2 = -4
- nums1[2] * nums2[0] = 0 * 2 = 0
- nums1[2] * nums2[1] = 0 * 4 = 0
Tích nhỏ thứ 6<sup>th</sup> là 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [-2,-1,0,1,2], nums2 = [-3,-1,2,4,5], k = 3
<strong>Đầu ra:</strong> -6
<strong>Giải thích:</strong> Ba tích nhỏ nhất là:
- nums1[0] * nums2[4] = (-2) * 5 = -10
- nums1[0] * nums2[3] = (-2) * 4 = -8
- nums1[4] * nums2[0] = 2 * (-3) = -6
Tích nhỏ thứ 3<sup>rd</sup> là -6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums1[i], nums2[j] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums1.length * nums2.length</code></li>
	<li><code>nums1</code> và <code>nums2</code> đã được sắp xếp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $2.5 \times 10^9$ tích nên không thể liệt kê chúng. Tích nhỏ thứ $k$ có tính đơn điệu theo ngưỡng $p$: số tích $\le p$ không bao giờ giảm.
>
> Ta tìm kiếm nhị phân $p$ trên đoạn $[-M,M]$. Với mỗi $x$ trong $nums1$, nếu x dương thì xét $nums2[i] \le p/x$, nếu x âm thì đảo chiều bất đẳng thức, còn nếu x = 0 thì toàn bộ mảng được tính khi $p \ge 0$. Vì các mảng đã sắp xếp, mỗi số lượng đều có thể được tính bằng tìm kiếm nhị phân.
>
> `bisect_left` trả về giá trị $p$ nhỏ nhất sao cho số lượng tích lớn hơn hoặc bằng $k$.

<!-- thinking:end -->

Ta có thể dùng tìm kiếm nhị phân để xác định giá trị tích $p$, với khoảng tìm kiếm là $[l, r]$, trong đó $l = -\textit{max}(|\textit{nums1}[0]|, |\textit{nums1}[n - 1]|) \times \textit{max}(|\textit{nums2}[0]|, |\textit{nums2}[n - 1]|)$ và $r = -l$.

Với mỗi $p$, ta tính số tích nhỏ hơn hoặc bằng $p$. Nếu số lượng này lớn hơn hoặc bằng $k$, tích nhỏ thứ $k$ chắc chắn nhỏ hơn hoặc bằng $p$, nên ta có thể thu hẹp đầu phải của khoảng về $p$. Nếu không, ta tăng đầu trái của khoảng lên $p + 1$.

Điểm mấu chốt của bài toán là cách tính số tích nhỏ hơn hoặc bằng $p$. Ta duyệt qua từng số $x$ trong $\textit{nums1}$ và xét các trường hợp:

- Nếu $x > 0$, thì $x \times \textit{nums2}[i]$ tăng dần khi $i$ tăng. Ta có thể dùng tìm kiếm nhị phân để tìm chỉ số $i$ nhỏ nhất sao cho $x \times \textit{nums2}[i] > p$. Khi đó, $i$ là số tích nhỏ hơn hoặc bằng $p$, được cộng dồn vào số lượng $\textit{cnt}$;
- Nếu $x < 0$, thì $x \times \textit{nums2}[i]$ giảm dần khi $i$ tăng. Ta có thể dùng tìm kiếm nhị phân để tìm chỉ số $i$ nhỏ nhất sao cho $x \times \textit{nums2}[i] \leq p$. Khi đó, $n - i$ là số tích nhỏ hơn hoặc bằng $p$, được cộng dồn vào số lượng $\textit{cnt}$;
- Nếu $x = 0$, thì $x \times \textit{nums2}[i] = 0$. Nếu $p \geq 0$, có $n$ tích nhỏ hơn hoặc bằng $p$, được cộng dồn vào số lượng $\textit{cnt}$.

Như vậy, ta có thể tìm tích nhỏ thứ $k$ bằng tìm kiếm nhị phân.

Độ phức tạp thời gian là $O(m \times \log n \times \log M)$, trong đó $m$ và $n$ lần lượt là độ dài của $\textit{nums1}$ và $\textit{nums2}$, còn $M$ là giá trị tuyệt đối lớn nhất trong $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthSmallestProduct(self, nums1: List[int], nums2: List[int], k: int) -> int:
        def count(p: int) -> int:
            cnt = 0
            n = len(nums2)
            for x in nums1:
                if x > 0:
                    cnt += bisect_right(nums2, p / x)
                elif x < 0:
                    cnt += n - bisect_left(nums2, p / x)
                else:
                    cnt += n * int(p >= 0)
            return cnt

        mx = max(abs(nums1[0]), abs(nums1[-1])) * max(abs(nums2[0]), abs(nums2[-1]))
        return bisect_left(range(-mx, mx + 1), k, key=count) - mx
```

#### Java

```java
class Solution {
    private int[] nums1;
    private int[] nums2;

    public long kthSmallestProduct(int[] nums1, int[] nums2, long k) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        int m = nums1.length;
        int n = nums2.length;
        int a = Math.max(Math.abs(nums1[0]), Math.abs(nums1[m - 1]));
        int b = Math.max(Math.abs(nums2[0]), Math.abs(nums2[n - 1]));
        long r = (long) a * b;
        long l = (long) -a * b;
        while (l < r) {
            long mid = (l + r) >> 1;
            if (count(mid) >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private long count(long p) {
        long cnt = 0;
        int n = nums2.length;
        for (int x : nums1) {
            if (x > 0) {
                int l = 0, r = n;
                while (l < r) {
                    int mid = (l + r) >> 1;
                    if ((long) x * nums2[mid] > p) {
                        r = mid;
                    } else {
                        l = mid + 1;
                    }
                }
                cnt += l;
            } else if (x < 0) {
                int l = 0, r = n;
                while (l < r) {
                    int mid = (l + r) >> 1;
                    if ((long) x * nums2[mid] <= p) {
                        r = mid;
                    } else {
                        l = mid + 1;
                    }
                }
                cnt += n - l;
            } else if (p >= 0) {
                cnt += n;
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long kthSmallestProduct(vector<int>& nums1, vector<int>& nums2, long long k) {
        int m = nums1.size(), n = nums2.size();
        int a = max(abs(nums1[0]), abs(nums1[m - 1]));
        int b = max(abs(nums2[0]), abs(nums2[n - 1]));
        long long r = 1LL * a * b;
        long long l = -r;
        auto count = [&](long long p) {
            long long cnt = 0;
            for (int x : nums1) {
                if (x > 0) {
                    int l = 0, r = n;
                    while (l < r) {
                        int mid = (l + r) >> 1;
                        if (1LL * x * nums2[mid] > p) {
                            r = mid;
                        } else {
                            l = mid + 1;
                        }
                    }
                    cnt += l;
                } else if (x < 0) {
                    int l = 0, r = n;
                    while (l < r) {
                        int mid = (l + r) >> 1;
                        if (1LL * x * nums2[mid] <= p) {
                            r = mid;
                        } else {
                            l = mid + 1;
                        }
                    }
                    cnt += n - l;
                } else if (p >= 0) {
                    cnt += n;
                }
            }
            return cnt;
        };
        while (l < r) {
            long long mid = (l + r) >> 1;
            if (count(mid) >= k) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func kthSmallestProduct(nums1 []int, nums2 []int, k int64) int64 {
	m := len(nums1)
	n := len(nums2)
	a := max(abs(nums1[0]), abs(nums1[m-1]))
	b := max(abs(nums2[0]), abs(nums2[n-1]))
	r := int64(a) * int64(b)
	l := -r

	count := func(p int64) int64 {
		var cnt int64
		for _, x := range nums1 {
			if x > 0 {
				l, r := 0, n
				for l < r {
					mid := (l + r) >> 1
					if int64(x)*int64(nums2[mid]) > p {
						r = mid
					} else {
						l = mid + 1
					}
				}
				cnt += int64(l)
			} else if x < 0 {
				l, r := 0, n
				for l < r {
					mid := (l + r) >> 1
					if int64(x)*int64(nums2[mid]) <= p {
						r = mid
					} else {
						l = mid + 1
					}
				}
				cnt += int64(n - l)
			} else if p >= 0 {
				cnt += int64(n)
			}
		}
		return cnt
	}

	for l < r {
		mid := (l + r) >> 1
		if count(mid) >= k {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l
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
function kthSmallestProduct(nums1: number[], nums2: number[], k: number): number {
    const m = nums1.length;
    const n = nums2.length;

    const a = BigInt(Math.max(Math.abs(nums1[0]), Math.abs(nums1[m - 1])));
    const b = BigInt(Math.max(Math.abs(nums2[0]), Math.abs(nums2[n - 1])));

    let l = -a * b;
    let r = a * b;

    const count = (p: bigint): bigint => {
        let cnt = 0n;
        for (const x of nums1) {
            const bx = BigInt(x);
            if (bx > 0n) {
                let l = 0,
                    r = n;
                while (l < r) {
                    const mid = (l + r) >> 1;
                    const prod = bx * BigInt(nums2[mid]);
                    if (prod > p) {
                        r = mid;
                    } else {
                        l = mid + 1;
                    }
                }
                cnt += BigInt(l);
            } else if (bx < 0n) {
                let l = 0,
                    r = n;
                while (l < r) {
                    const mid = (l + r) >> 1;
                    const prod = bx * BigInt(nums2[mid]);
                    if (prod <= p) {
                        r = mid;
                    } else {
                        l = mid + 1;
                    }
                }
                cnt += BigInt(n - l);
            } else if (p >= 0n) {
                cnt += BigInt(n);
            }
        }
        return cnt;
    };

    while (l < r) {
        const mid = (l + r) >> 1n;
        if (count(mid) >= BigInt(k)) {
            r = mid;
        } else {
            l = mid + 1n;
        }
    }

    return Number(l);
}
```

#### Rust

```rust
impl Solution {
    pub fn kth_smallest_product(nums1: Vec<i32>, nums2: Vec<i32>, k: i64) -> i64 {
        let m = nums1.len();
        let n = nums2.len();
        let a = nums1[0].abs().max(nums1[m - 1].abs()) as i64;
        let b = nums2[0].abs().max(nums2[n - 1].abs()) as i64;
        let mut l = -a * b;
        let mut r = a * b;

        let count = |p: i64| -> i64 {
            let mut cnt = 0i64;
            for &x in &nums1 {
                if x > 0 {
                    let mut left = 0;
                    let mut right = n;
                    while left < right {
                        let mid = (left + right) / 2;
                        if (x as i64) * (nums2[mid] as i64) > p {
                            right = mid;
                        } else {
                            left = mid + 1;
                        }
                    }
                    cnt += left as i64;
                } else if x < 0 {
                    let mut left = 0;
                    let mut right = n;
                    while left < right {
                        let mid = (left + right) / 2;
                        if (x as i64) * (nums2[mid] as i64) <= p {
                            right = mid;
                        } else {
                            left = mid + 1;
                        }
                    }
                    cnt += (n - left) as i64;
                } else if p >= 0 {
                    cnt += n as i64;
                }
            }
            cnt
        };

        while l < r {
            let mid = l + (r - l) / 2;
            if count(mid) >= k {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
