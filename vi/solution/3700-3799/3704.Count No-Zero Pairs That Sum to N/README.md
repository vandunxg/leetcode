---
comments: true
difficulty: Hard
rating: 2419
source: Weekly Contest 470 Q4
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [3704. Count No-Zero Pairs That Sum to N](https://leetcode.com/problems/count-no-zero-pairs-that-sum-to-n)

[中文文档](/solution/3700-3799/3704.Count%20No-Zero%20Pairs%20That%20Sum%20to%20N/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên <strong>không chứa số 0</strong> là một số nguyên <strong>dương</strong> <strong>không chứa chữ số</strong> 0 trong biểu diễn thập phân.</p>

<p>Cho một số nguyên <code>n</code>, hãy đếm số cặp <code>(a, b)</code> thỏa mãn:</p>

<ul>
	<li><code>a</code> và <code>b</code> là các số nguyên <strong>không chứa số 0</strong>.</li>
	<li><code>a + b = n</code></li>
</ul>

<p>Trả về một số nguyên biểu thị số lượng cặp như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cặp duy nhất là <code>(1, 1)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp là <code>(1, 2)</code> và <code>(2, 1)</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 11</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp là <code>(2, 9)</code>, <code>(3, 8)</code>, <code>(4, 7)</code>, <code>(5, 6)</code>, <code>(6, 5)</code>, <code>(7, 4)</code>, <code>(8, 3)</code> và <code>(9, 2)</code>. Lưu ý rằng <code>(1, 10)</code> và <code>(10, 1)</code> không thỏa mãn điều kiện vì 10 chứa số 0 trong biểu diễn thập phân.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê $a$ rồi kiểm tra xem $b=n-a$ có chứa chữ số 0 hay không là bất khả thi khi $n$ lớn. Phép cộng diễn ra theo từng chữ số với một carry; cả hai số hạng đều không được chứa số $0$ và độ dài của chúng có thể khác nhau. Digit DP từ hàng thấp lên hàng cao sẽ theo dõi carry và việc mỗi số còn tiếp tục hay không. Ta thêm một số 0 ở đầu để hấp thụ carry cuối cùng, vì vậy carry cuối phải bằng $0$.

<!-- thinking:end -->

Ta thực hiện digit DP trên biểu diễn thập phân của $n$, từ chữ số ít quan trọng nhất đến chữ số quan trọng nhất.

Trạng thái: `dp[pos][carry][aliveA][aliveB]` = số cách đối với phần hậu tố đã được xử lý.

- `carry` là carry đi vào chữ số hiện tại (0 hoặc 1).
- `aliveA/aliveB` cho biết số tương ứng còn chữ số ở các vị trí cao hơn hay không. Nếu `aliveX = 0`, tất cả các chữ số cao hơn còn lại phải là các số 0 đứng đầu (chữ số 0 này không thuộc biểu diễn thập phân).

Chuyển trạng thái: chọn các chữ số `da` và `db`:

- Nếu `aliveX = 1`, chữ số thuộc `[1..9]` (không chứa số 0).
- Ngược lại, chữ số là `0`.

Chúng phải thỏa mãn `(da + db + carry) % 10 == digit_n[pos]`. Sau đó, `aliveA`/`aliveB` có thể tiếp tục bằng `1` hoặc trở thành `0` (kết thúc số tại chữ số này).

Ta thêm một chữ số `0` ở đầu $n$ để xử lý hoàn toàn carry cuối cùng. Đáp án là `dp[last][0][0][0]`.

Độ phức tạp thời gian là $O(L \cdot 9^2)$ và độ phức tạp không gian là $O(1)$, trong đó $L$ là số chữ số của $n$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
	def countNoZeroPairs(self, n: int) -> int:
		digits = list(map(int, str(n)))[::-1]
		digits.append(0)  # absorb final carry
		L = len(digits)

		# dp[carry][aliveA][aliveB]
		dp = [[[0] * 2 for _ in range(2)] for _ in range(2)]
		dp[0][1][1] = 1

		for pos in range(L):
			ndp = [[[0] * 2 for _ in range(2)] for _ in range(2)]
			target = digits[pos]
			for carry in range(2):
				for aliveA in range(2):
					for aliveB in range(2):
						ways = dp[carry][aliveA][aliveB]
						if ways == 0:
							continue

						if aliveA:
							A = [(d, 1) for d in range(1, 10)]
							if pos > 0:
								A.append((0, 0))  # end number here
						else:
							A = [(0, 0)]

						if aliveB:
							B = [(d, 1) for d in range(1, 10)]
							if pos > 0:
								B.append((0, 0))
						else:
							B = [(0, 0)]

						for da, na in A:
							for db, nb in B:
								s = da + db + carry
								if s % 10 != target:
									continue
								ndp[s // 10][na][nb] += ways
			dp = ndp

		return dp[0][0][0]

```

#### Java

```java
class Solution {
	public long countNoZeroPairs(long n) {
		char[] cs = Long.toString(n).toCharArray();
		int m = cs.length;
		int[] digits = new int[m + 1];
		for (int i = 0; i < m; i++) {
			digits[i] = cs[m - 1 - i] - '0';
		}
		digits[m] = 0;

		long[][][] dp = new long[2][2][2];
		dp[0][1][1] = 1;

		for (int pos = 0; pos < m + 1; pos++) {
			long[][][] ndp = new long[2][2][2];
			int target = digits[pos];
			for (int carry = 0; carry <= 1; carry++) {
				for (int aliveA = 0; aliveA <= 1; aliveA++) {
					for (int aliveB = 0; aliveB <= 1; aliveB++) {
						long ways = dp[carry][aliveA][aliveB];
						if (ways == 0) {
							continue;
						}
						int[] aDigits;
						int[] aNext;
						if (aliveA == 1) {
							if (pos == 0) {
								aDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9};
								aNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1};
							} else {
								aDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
								aNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1, 0};
							}
						} else {
							aDigits = new int[] {0};
							aNext = new int[] {0};
						}

						int[] bDigits;
						int[] bNext;
						if (aliveB == 1) {
							if (pos == 0) {
								bDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9};
								bNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1};
							} else {
								bDigits = new int[] {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
								bNext = new int[] {1, 1, 1, 1, 1, 1, 1, 1, 1, 0};
							}
						} else {
							bDigits = new int[] {0};
							bNext = new int[] {0};
						}

						for (int ai = 0; ai < aDigits.length; ai++) {
							int da = aDigits[ai];
							int na = aNext[ai];
							for (int bi = 0; bi < bDigits.length; bi++) {
								int db = bDigits[bi];
								int nb = bNext[bi];
								int s = da + db + carry;
								if (s % 10 != target) {
									continue;
								}
								int ncarry = s / 10;
								ndp[ncarry][na][nb] += ways;
							}
						}
					}
				}
			}
			dp = ndp;
		}

		return dp[0][0][0];
	}
}

```

#### C++

```cpp
class Solution {
public:
	long long countNoZeroPairs(long long n) {
		std::string s = std::to_string(n);
		int m = (int) s.size();
		std::vector<int> digits(m + 1);
		for (int i = 0; i < m; i++) {
			digits[i] = s[m - 1 - i] - '0';
		}
		digits[m] = 0;

		long long dp[2][2][2] = {};
		dp[0][1][1] = 1;

		for (int pos = 0; pos < m + 1; pos++) {
			long long ndp[2][2][2] = {};
			int target = digits[pos];
			for (int carry = 0; carry <= 1; carry++) {
				for (int aliveA = 0; aliveA <= 1; aliveA++) {
					for (int aliveB = 0; aliveB <= 1; aliveB++) {
						long long ways = dp[carry][aliveA][aliveB];
						if (!ways) continue;
						int aDigits[10];
						int aNext[10];
						int aLen = 0;
						if (aliveA) {
							for (int d = 1; d <= 9; d++) {
								aDigits[aLen] = d;
								aNext[aLen] = 1;
								aLen++;
							}
							if (pos > 0) {
								aDigits[aLen] = 0;
								aNext[aLen] = 0;
								aLen++;
							}
						} else {
							aDigits[0] = 0;
							aNext[0] = 0;
							aLen = 1;
						}

						int bDigits[10];
						int bNext[10];
						int bLen = 0;
						if (aliveB) {
							for (int d = 1; d <= 9; d++) {
								bDigits[bLen] = d;
								bNext[bLen] = 1;
								bLen++;
							}
							if (pos > 0) {
								bDigits[bLen] = 0;
								bNext[bLen] = 0;
								bLen++;
							}
						} else {
							bDigits[0] = 0;
							bNext[0] = 0;
							bLen = 1;
						}

						for (int ia = 0; ia < aLen; ia++) {
							int da = aDigits[ia];
							int na = aNext[ia];
							for (int ib = 0; ib < bLen; ib++) {
								int db = bDigits[ib];
								int nb = bNext[ib];
								int sum = da + db + carry;
								if (sum % 10 != target) continue;
								int ncarry = sum / 10;
								ndp[ncarry][na][nb] += ways;
							}
						}
					}
				}
			}
			std::memcpy(dp, ndp, sizeof(dp));
		}

		return dp[0][0][0];
	}
};

```

#### Go

```go
package main

import "strconv"

func countNoZeroPairs(n int64) int64 {
	s := []byte(strconv.FormatInt(n, 10))
	m := len(s)
	digits := make([]int, m+1)
	for i := 0; i < m; i++ {
		digits[i] = int(s[m-1-i] - '0')
	}
	digits[m] = 0

	var dp [2][2][2]int64
	dp[0][1][1] = 1

	for pos := 0; pos < m+1; pos++ {
		var ndp [2][2][2]int64
		target := digits[pos]
		for carry := 0; carry <= 1; carry++ {
			for aliveA := 0; aliveA <= 1; aliveA++ {
				for aliveB := 0; aliveB <= 1; aliveB++ {
					ways := dp[carry][aliveA][aliveB]
					if ways == 0 {
						continue
					}
					var A [10][2]int
					aLen := 0
					if aliveA == 1 {
						for d := 1; d <= 9; d++ {
							A[aLen][0] = d
							A[aLen][1] = 1
							aLen++
						}
						if pos > 0 {
							A[aLen][0] = 0
							A[aLen][1] = 0
							aLen++
						}
					} else {
						A[0][0] = 0
						A[0][1] = 0
						aLen = 1
					}

					var B [10][2]int
					bLen := 0
					if aliveB == 1 {
						for d := 1; d <= 9; d++ {
							B[bLen][0] = d
							B[bLen][1] = 1
							bLen++
						}
						if pos > 0 {
							B[bLen][0] = 0
							B[bLen][1] = 0
							bLen++
						}
					} else {
						B[0][0] = 0
						B[0][1] = 0
						bLen = 1
					}

					for ai := 0; ai < aLen; ai++ {
						da, na := A[ai][0], A[ai][1]
						for bi := 0; bi < bLen; bi++ {
							db, nb := B[bi][0], B[bi][1]
							sum := da + db + carry
							if sum%10 != target {
								continue
							}
							ncarry := sum / 10
							ndp[ncarry][na][nb] += ways
						}
					}
				}
			}
		}
		dp = ndp
	}

	return dp[0][0][0]
}

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
