---
comments: true
difficulty: Medium
rating: 1361
source: Weekly Contest 279 Q2
tags:
    - Math
    - Sorting
---

<!-- problem:start -->

# [2165. Smallest Value of the Rearranged Number](https://leetcode.com/problems/smallest-value-of-the-rearranged-number)

[中文文档](/solution/2100-2199/2165.Smallest%20Value%20of%20the%20Rearranged%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>num.</code> Hãy <strong>sắp xếp lại</strong> các chữ số của <code>num</code> sao cho giá trị của nó <strong>nhỏ nhất</strong> và không chứa <strong>bất kỳ</strong> số 0 nào ở đầu.</p>

<p>Trả về <em>số sau khi sắp xếp lại có giá trị nhỏ nhất</em>.</p>

<p>Lưu ý rằng dấu của số không thay đổi sau khi sắp xếp lại các chữ số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 310
<strong>Đầu ra:</strong> 103
<strong>Giải thích:</strong> Các cách sắp xếp có thể có của các chữ số trong 310 là 013, 031, 103, 130, 301, 310.
Cách sắp xếp có giá trị nhỏ nhất mà không chứa số 0 ở đầu là 103.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = -7605
<strong>Đầu ra:</strong> -7650
<strong>Giải thích:</strong> Một số cách sắp xếp có thể có của các chữ số trong -7605 là -7650, -6705, -5076, -0567.
Cách sắp xếp có giá trị nhỏ nhất mà không chứa số 0 ở đầu là -7650.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-10<sup>15</sup> &lt;= num &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta sắp xếp lại các chữ số: số âm phải có giá trị nhỏ nhất có thể (các chữ số theo thứ tự giảm dần), còn số dương phải nhỏ nhất có thể nhưng không được có số 0 ở đầu. Không cần xét tất cả các hoán vị.
>
> Đếm số lần xuất hiện của các chữ số $0$– $9$. Với giá trị âm, đưa các chữ số từ $9$ xuống $0$ vào kết quả; với giá trị dương, đặt chữ số khác 0 nhỏ nhất ở đầu, sau đó đặt các chữ số còn lại theo thứ tự tăng dần (bao gồm cả số 0).
>
> Đếm trên giá trị tuyệt đối rồi khôi phục dấu.

<!-- thinking:end -->

Trước tiên, ta dùng một mảng $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi chữ số trong $\textit{num}$.

Nếu $\textit{num}$ là số âm, các chữ số cần được sắp xếp theo thứ tự giảm dần. Vì vậy, ta duyệt $\textit{cnt}$ từ $9$ đến $0$ và sắp xếp các chữ số theo thứ tự giảm dần dựa trên số lần xuất hiện của chúng.

Nếu $\textit{num}$ là số dương, trước tiên ta tìm chữ số khác 0 đầu tiên và đặt nó ở vị trí đầu tiên, sau đó sắp xếp các chữ số còn lại theo thứ tự tăng dần.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là kích thước của số $\textit{num}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestNumber(self, num: int) -> int:
        neg = num < 0
        num = abs(num)
        cnt = [0] * 10
        while num:
            cnt[num % 10] += 1
            num //= 10
        ans = 0
        if neg:
            for i in reversed(range(10)):
                for _ in range(cnt[i]):
                    ans *= 10
                    ans += i
            return -ans
        if cnt[0]:
            for i in range(1, 10):
                if cnt[i]:
                    ans = i
                    cnt[i] -= 1
                    break
        for i in range(10):
            for _ in range(cnt[i]):
                ans *= 10
                ans += i
        return ans
```

#### Java

```java
class Solution {
    public long smallestNumber(long num) {
        boolean neg = num < 0;
        num = Math.abs(num);
        int[] cnt = new int[10];
        while (num > 0) {
            ++cnt[(int) (num % 10)];
            num /= 10;
        }
        long ans = 0;
        if (neg) {
            for (int i = 9; i >= 0; --i) {
                while (cnt[i] > 0) {
                    ans = ans * 10 + i;
                    --cnt[i];
                }
            }
            return -ans;
        }
        if (cnt[0] > 0) {
            for (int i = 1; i < 10; ++i) {
                if (cnt[i] > 0) {
                    --cnt[i];
                    ans = i;
                    break;
                }
            }
        }
        for (int i = 0; i < 10; ++i) {
            while (cnt[i] > 0) {
                ans = ans * 10 + i;
                --cnt[i];
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
    long long smallestNumber(long long num) {
        bool neg = num < 0;
        num = abs(num);
        int cnt[10]{};
        while (num > 0) {
            ++cnt[num % 10];
            num /= 10;
        }
        long long ans = 0;
        if (neg) {
            for (int i = 9; i >= 0; --i) {
                while (cnt[i] > 0) {
                    ans = ans * 10 + i;
                    --cnt[i];
                }
            }
            return -ans;
        }
        if (cnt[0]) {
            for (int i = 1; i < 10; ++i) {
                if (cnt[i] > 0) {
                    --cnt[i];
                    ans = i;
                    break;
                }
            }
        }
        for (int i = 0; i < 10; ++i) {
            while (cnt[i] > 0) {
                ans = ans * 10 + i;
                --cnt[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func smallestNumber(num int64) (ans int64) {
	neg := num < 0
	num = max(num, -num)
	cnt := make([]int, 10)

	for num > 0 {
		cnt[num%10]++
		num /= 10
	}

	if neg {
		for i := 9; i >= 0; i-- {
			for cnt[i] > 0 {
				ans = ans*10 + int64(i)
				cnt[i]--
			}
		}
		return -ans
	}

	if cnt[0] > 0 {
		for i := 1; i < 10; i++ {
			if cnt[i] > 0 {
				cnt[i]--
				ans = int64(i)
				break
			}
		}
	}

	for i := 0; i < 10; i++ {
		for cnt[i] > 0 {
			ans = ans*10 + int64(i)
			cnt[i]--
		}
	}

	return ans
}
```

#### TypeScript

```ts
function smallestNumber(num: number): number {
    const neg = num < 0;
    num = Math.abs(num);
    const cnt = Array(10).fill(0);

    while (num > 0) {
        cnt[num % 10]++;
        num = Math.floor(num / 10);
    }

    let ans = 0;
    if (neg) {
        for (let i = 9; i >= 0; i--) {
            while (cnt[i] > 0) {
                ans = ans * 10 + i;
                cnt[i]--;
            }
        }
        return -ans;
    }

    if (cnt[0] > 0) {
        for (let i = 1; i < 10; i++) {
            if (cnt[i] > 0) {
                cnt[i]--;
                ans = i;
                break;
            }
        }
    }

    for (let i = 0; i < 10; i++) {
        while (cnt[i] > 0) {
            ans = ans * 10 + i;
            cnt[i]--;
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_number(num: i64) -> i64 {
        let mut neg = num < 0;
        let mut num = num.abs();
        let mut cnt = vec![0; 10];

        while num > 0 {
            cnt[(num % 10) as usize] += 1;
            num /= 10;
        }

        let mut ans = 0;
        if neg {
            for i in (0..10).rev() {
                while cnt[i] > 0 {
                    ans = ans * 10 + i as i64;
                    cnt[i] -= 1;
                }
            }
            return -ans;
        }

        if cnt[0] > 0 {
            for i in 1..10 {
                if cnt[i] > 0 {
                    cnt[i] -= 1;
                    ans = i as i64;
                    break;
                }
            }
        }

        for i in 0..10 {
            while cnt[i] > 0 {
                ans = ans * 10 + i as i64;
                cnt[i] -= 1;
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {number}
 */
var smallestNumber = function (num) {
    const neg = num < 0;
    num = Math.abs(num);
    const cnt = Array(10).fill(0);

    while (num > 0) {
        cnt[num % 10]++;
        num = Math.floor(num / 10);
    }

    let ans = 0;
    if (neg) {
        for (let i = 9; i >= 0; i--) {
            while (cnt[i] > 0) {
                ans = ans * 10 + i;
                cnt[i]--;
            }
        }
        return -ans;
    }

    if (cnt[0] > 0) {
        for (let i = 1; i < 10; i++) {
            if (cnt[i] > 0) {
                cnt[i]--;
                ans = i;
                break;
            }
        }
    }

    for (let i = 0; i < 10; i++) {
        while (cnt[i] > 0) {
            ans = ans * 10 + i;
            cnt[i]--;
        }
    }

    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
