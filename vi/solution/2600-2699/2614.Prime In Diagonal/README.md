---
comments: true
difficulty: Easy
rating: 1375
source: Weekly Contest 340 Q1
tags:
    - Array
    - Math
    - Matrix
    - Number Theory
---

<!-- problem:start -->

# [2614. Prime In Diagonal](https://leetcode.com/problems/prime-in-diagonal)

[中文文档](/solution/2600-2699/2614.Prime%20In%20Diagonal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên hai chiều <code>nums</code>, đánh chỉ số từ 0.</p>

<p>Trả về <em>số <strong>nguyên tố</strong> lớn nhất nằm trên ít nhất một trong các <b>đường chéo</b> của </em><code>nums</code>. Nếu không có số nguyên tố nào trên các đường chéo, trả về<em> 0.</em></p>

<p>Lưu ý:</p>

<ul>
	<li>Một số nguyên là <strong>số nguyên tố</strong> nếu nó lớn hơn <code>1</code> và không có ước nguyên dương nào ngoài <code>1</code> và chính nó.</li>
	<li>Một số nguyên <code>val</code> nằm trên một trong các <strong>đường chéo</strong> của <code>nums</code> nếu tồn tại một số nguyên <code>i</code> sao cho <code>nums[i][i] = val</code> hoặc một <code>i</code> sao cho <code>nums[i][nums.length - i - 1] = val</code>.</li>
</ul>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2614.Prime%20In%20Diagonal/images/screenshot-2023-03-06-at-45648-pm.png" style="width: 181px; height: 121px;" /></p>

<p>Trong hình trên, một đường chéo là <strong>[1,5,9]</strong> và đường chéo còn lại là<strong> [3,5,7]</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1,2,3],[5,6,7],[9,10,11]]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Các số 1, 3, 6, 9 và 11 là những số duy nhất xuất hiện trên ít nhất một trong các đường chéo. Vì 11 là số nguyên tố lớn nhất, ta trả về 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1,2,3],[5,17,7],[9,11,10]]
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Các số 1, 3, 9, 10 và 17 đều xuất hiện trên ít nhất một trong các đường chéo. 17 là số nguyên tố lớn nhất, nên ta trả về 17.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 300</code></li>
	<li><code>nums.length == nums<sub>i</sub>.length</code></li>
	<li><code>1 &lt;= nums<span style="font-size: 10.8333px;">[i][j]</span>&nbsp;&lt;= 4*10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần quan tâm đến hai đường chéo. Ma trận có kích thước tối đa $300$ và các giá trị tối đa $4\times 10^6$, nên kiểm tra chia thử trên các phần tử đường chéo với độ phức tạp $O(n\sqrt{M})$ là đủ.
>
> Các số nguyên nhỏ hơn $2$ không phải là số nguyên tố; với các số còn lại, ta kiểm tra các ước đến căn bậc hai. Ta duyệt qua cả hai đường chéo và giữ lại số nguyên tố lớn nhất.

<!-- thinking:end -->

Ta cài đặt hàm `is_prime` để kiểm tra một số có phải là số nguyên tố hay không.

Sau đó, ta duyệt qua mảng và kiểm tra các số trên hai đường chéo có phải là số nguyên tố hay không. Nếu đúng, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n \times \sqrt{M})$, trong đó $n$ và $M$ lần lượt là số hàng của mảng và giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def diagonalPrime(self, nums: List[List[int]]) -> int:
        def is_prime(x: int) -> bool:
            if x < 2:
                return False
            return all(x % i for i in range(2, int(sqrt(x)) + 1))

        n = len(nums)
        ans = 0
        for i, row in enumerate(nums):
            if is_prime(row[i]):
                ans = max(ans, row[i])
            if is_prime(row[n - i - 1]):
                ans = max(ans, row[n - i - 1])
        return ans
```

#### Java

```java
class Solution {
    public int diagonalPrime(int[][] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (isPrime(nums[i][i])) {
                ans = Math.max(ans, nums[i][i]);
            }
            if (isPrime(nums[i][n - i - 1])) {
                ans = Math.max(ans, nums[i][n - i - 1]);
            }
        }
        return ans;
    }

    private boolean isPrime(int x) {
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
}
```

#### C++

```cpp
class Solution {
public:
    int diagonalPrime(vector<vector<int>>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (isPrime(nums[i][i])) {
                ans = max(ans, nums[i][i]);
            }
            if (isPrime(nums[i][n - i - 1])) {
                ans = max(ans, nums[i][n - i - 1]);
            }
        }
        return ans;
    }

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
func diagonalPrime(nums [][]int) (ans int) {
	n := len(nums)
	for i, row := range nums {
		if isPrime(row[i]) {
			ans = max(ans, row[i])
		}
		if isPrime(row[n-i-1]) {
			ans = max(ans, row[n-i-1])
		}
	}
	return
}

func isPrime(x int) bool {
	if x < 2 {
		return false
	}
	for i := 2; i <= x/i; i++ {
		if x%i == 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function diagonalPrime(nums: number[][]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (isPrime(nums[i][i])) {
            ans = Math.max(ans, nums[i][i]);
        }
        if (isPrime(nums[i][n - i - 1])) {
            ans = Math.max(ans, nums[i][n - i - 1]);
        }
    }
    return ans;
}

function isPrime(x: number): boolean {
    if (x < 2) {
        return false;
    }
    for (let i = 2; i <= Math.floor(x / i); ++i) {
        if (x % i === 0) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn diagonal_prime(nums: Vec<Vec<i32>>) -> i32 {
        let mut ans = 0;
        let n = nums.len();

        for (i, row) in nums.iter().enumerate() {
            if Self::is_prime(row[i]) && row[i] > ans {
                ans = row[i];
            }
            if Self::is_prime(row[n - i - 1]) && row[n - i - 1] > ans {
                ans = row[n - i - 1];
            }
        }

        ans
    }

    fn is_prime(n: i32) -> bool {
        if n < 2 {
            return false;
        }

        let upper = (n as f64).sqrt() as i32;
        for i in 2..=upper {
            if n % i == 0 {
                return false;
            }
        }

        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} nums
 * @return {number}
 */
var diagonalPrime = function (nums) {
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < n; i++) {
        if (isPrime(nums[i][i])) {
            ans = Math.max(ans, nums[i][i]);
        }
        if (isPrime(nums[i][n - i - 1])) {
            ans = Math.max(ans, nums[i][n - i - 1]);
        }
    }
    return ans;
};

function isPrime(x) {
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
