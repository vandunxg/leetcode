---
comments: true
difficulty: Easy
rating: 1234
source: Weekly Contest 455 Q1
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Number Theory
---

<!-- problem:start -->

# [3591. Check if Any Element Has Prime Frequency](https://leetcode.com/problems/check-if-any-element-has-prime-frequency)

[中文文档](/solution/3500-3599/3591.Check%20if%20Any%20Element%20Has%20Prime%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trả về <code>true</code> nếu tần suất của bất kỳ phần tử nào trong mảng là <strong>số nguyên tố</strong>; nếu không, trả về <code>false</code>.</p>

<p><strong>Tần suất</strong> của một phần tử <code>x</code> là số lần phần tử đó xuất hiện trong mảng.</p>

<p>Số nguyên tố là một số tự nhiên lớn hơn 1 chỉ có hai ước, 1 và chính nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>4 có tần suất là hai, đây là một số nguyên tố.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả các phần tử đều có tần suất là một.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,2,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cả 2 và 4 đều có tần suất là một số nguyên tố.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Kiểm tra số nguyên tố

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần biết có giá trị nào có tần suất là số nguyên tố hay không. Trước tiên đếm tần suất, sau đó thử chia từng tần suất.
>
> Tần suất không vượt quá $n$, nên chỉ cần kiểm tra đến căn bậc hai của nó. Trả về true ngay khi gặp tần suất là số nguyên tố.

<!-- thinking:end -->

Ta dùng một hash table $\text{cnt}$ để đếm tần suất của mỗi phần tử. Sau đó, ta duyệt qua các giá trị trong $\text{cnt}$ và kiểm tra xem có giá trị nào là số nguyên tố hay không. Nếu có số nguyên tố, trả về `true`; nếu không, trả về `false`.

Độ phức tạp thời gian là $O(n \times \sqrt{M})$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$ và $M$ là giá trị lớn nhất

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkPrimeFrequency(self, nums: List[int]) -> bool:
        def is_prime(x: int) -> bool:
            if x < 2:
                return False
            return all(x % i for i in range(2, int(sqrt(x)) + 1))

        cnt = Counter(nums)
        return any(is_prime(x) for x in cnt.values())
```

#### Java

```java
import java.util.*;

class Solution {
    public boolean checkPrimeFrequency(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }

        for (int x : cnt.values()) {
            if (isPrime(x)) {
                return true;
            }
        }
        return false;
    }

    private boolean isPrime(int x) {
        if (x < 2) {
            return false;
        }
        for (int i = 2; i <= x / i; i++) {
            if (x % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkPrimeFrequency(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }

        for (auto& [_, x] : cnt) {
            if (isPrime(x)) {
                return true;
            }
        }
        return false;
    }

private:
    bool isPrime(int x) {
        if (x < 2) {
            return false;
        }
        for (int i = 2; i <= x / i; ++i) {
            if (x % i == 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkPrimeFrequency(nums []int) bool {
	cnt := make(map[int]int)
	for _, x := range nums {
		cnt[x]++
	}
	for _, x := range cnt {
		if isPrime(x) {
			return true
		}
	}
	return false
}

func isPrime(x int) bool {
	if x < 2 {
		return false
	}
	for i := 2; i*i <= x; i++ {
		if x%i == 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function checkPrimeFrequency(nums: number[]): boolean {
    const cnt: Record<number, number> = {};
    for (const x of nums) {
        cnt[x] = (cnt[x] || 0) + 1;
    }
    for (const x of Object.values(cnt)) {
        if (isPrime(x)) {
            return true;
        }
    }
    return false;
}

function isPrime(x: number): boolean {
    if (x < 2) {
        return false;
    }
    for (let i = 2; i * i <= x; i++) {
        if (x % i === 0) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
