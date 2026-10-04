---
comments: true
difficulty: Medium
rating: 1737
source: Weekly Contest 385 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Enumeration
    - Matrix
    - Number Theory
    - Primality Test
    - Sieve
    - Sieve of Eratosthenes
---

<!-- problem:start -->

# [3044. Most Frequent Prime](https://leetcode.com/problems/most-frequent-prime)

[中文文档](/solution/3000-3099/3044.Most%20Frequent%20Prime/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một ma trận 2D <code>m x n</code> <strong>được đánh chỉ số từ 0</strong><strong> </strong><code>mat</code>. Từ mỗi ô, ta có thể tạo ra các số theo cách sau:</p>

<ul>
	<li>Từ mỗi ô có nhiều nhất <code>8</code> hướng di chuyển: đông, đông nam, nam, tây nam, tây, tây bắc, bắc và đông bắc.</li>
	<li>Chọn một hướng trong số đó rồi nối các chữ số trên đường đi vào số đang được tạo theo hướng đã chọn.</li>
	<li>Lưu ý rằng các số được tạo ra ở mỗi bước. Ví dụ, nếu các chữ số trên đường đi là <code>1, 9, 1</code>, thì ta tạo được ba số: <code>1, 19, 191</code>.</li>
</ul>

<p>Hãy trả về <em><span data-keyword="prime-number">số nguyên tố</span> <strong>lớn hơn</strong> </em><code>10</code><em> xuất hiện nhiều nhất </em>trong tất cả các số được tạo ra khi duyệt ma trận, hoặc <code>-1</code><em> nếu không tồn tại số nguyên tố nào như vậy. Nếu có nhiều số nguyên tố có cùng tần suất cao nhất, hãy trả về <b>số lớn nhất</b> trong số đó.</em></p>

<p><strong>Lưu ý:</strong> Không được thay đổi hướng trong quá trình di chuyển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3044.Most%20Frequent%20Prime/images/south" style="width: 641px; height: 291px;" /> </strong>

<pre>
<strong>
Đầu vào:</strong> mat = [[1,1],[9,9],[1,1]]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong>
Từ ô (0,0), có 3 hướng khả dĩ và các số lớn hơn 10 có thể tạo ra theo những hướng đó là:
Đông: [11], Đông nam: [19], Nam: [19,191].
Các số lớn hơn 10 tạo được từ ô (0,1) theo mọi hướng là: [19,191,19,11].
Các số lớn hơn 10 tạo được từ ô (1,0) theo mọi hướng là: [99,91,91,91,91].
Các số lớn hơn 10 tạo được từ ô (1,1) theo mọi hướng là: [91,91,99,91,91].
Các số lớn hơn 10 tạo được từ ô (2,0) theo mọi hướng là: [11,19,191,19].
Các số lớn hơn 10 tạo được từ ô (2,1) theo mọi hướng là: [11,19,19,191].
Số nguyên tố xuất hiện nhiều nhất trong tất cả các số được tạo ra là 19.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[7]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Số duy nhất có thể tạo ra là 7. Đây là số nguyên tố, nhưng không lớn hơn 10, nên trả về -1.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[9,7,8],[4,6,5],[2,8,6]]
<strong>Đầu ra:</strong> 97
<strong>Giải thích:</strong>
Các số lớn hơn 10 tạo được từ ô (0,0) theo mọi hướng là: [97,978,96,966,94,942].
Các số lớn hơn 10 tạo được từ ô (0,1) theo mọi hướng là: [78,75,76,768,74,79].
Các số lớn hơn 10 tạo được từ ô (0,2) theo mọi hướng là: [85,856,86,862,87,879].
Các số lớn hơn 10 tạo được từ ô (1,0) theo mọi hướng là: [46,465,48,42,49,47].
Các số lớn hơn 10 tạo được từ ô (1,1) theo mọi hướng là: [65,66,68,62,64,69,67,68].
Các số lớn hơn 10 tạo được từ ô (1,2) theo mọi hướng là: [56,58,56,564,57,58].
Các số lớn hơn 10 tạo được từ ô (2,0) theo mọi hướng là: [28,286,24,249,26,268].
Các số lớn hơn 10 tạo được từ ô (2,1) theo mọi hướng là: [86,82,84,86,867,85].
Các số lớn hơn 10 tạo được từ ô (2,2) theo mọi hướng là: [68,682,66,669,65,658].
Số nguyên tố xuất hiện nhiều nhất trong tất cả các số được tạo ra là 97.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 6</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ma trận có kích thước tối đa $6 \times 6$, nên số lượng số được tạo từ mỗi ô theo tám hướng là không nhiều. Ta cần tìm số nguyên tố lớn hơn $10$ xuất hiện nhiều nhất, nếu hòa thì chọn số lớn hơn.
>
> Ta có thể liệt kê các hướng và số bước; mỗi số tạo được sẽ được kiểm tra tính nguyên tố bằng phép chia thử rồi đếm tần suất.
>
> Cuối cùng, duyệt qua các tần suất để chọn số có tần suất và giá trị tốt nhất, hoặc trả về $-1$ nếu không có số nguyên tố nào.

<!-- thinking:end -->

Ta có thể dùng hash table để đếm tần suất của mỗi số nguyên tố lớn hơn 10.

Với mỗi ô, ta bắt đầu từ ô đó, tạo một số theo một trong 8 hướng, rồi kiểm tra xem số vừa tạo có phải là số nguyên tố lớn hơn 10 hay không. Nếu đúng, ta thêm số đó vào hash table.

Cuối cùng, ta duyệt qua hash table để tìm số nguyên tố có tần suất cao nhất. Nếu có nhiều số nguyên tố có cùng tần suất cao nhất, ta trả về số lớn nhất trong số đó.

Độ phức tạp thời gian là $O(m \times n \times \max(m, n) \times {10}^{\frac{\max(m, n)}{2}})$, và độ phức tạp không gian là $O(m \times n \times \max(m, n))$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của `mat`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostFrequentPrime(self, mat: List[List[int]]) -> int:
        def is_prime(x: int) -> int:
            return all(x % i != 0 for i in range(2, isqrt(x) + 1))

        m, n = len(mat), len(mat[0])
        cnt = Counter()
        for i in range(m):
            for j in range(n):
                for a in range(-1, 2):
                    for b in range(-1, 2):
                        if a == 0 and b == 0:
                            continue
                        x, y, v = i + a, j + b, mat[i][j]
                        while 0 <= x < m and 0 <= y < n:
                            v = v * 10 + mat[x][y]
                            if is_prime(v):
                                cnt[v] += 1
                            x, y = x + a, y + b
        ans, mx = -1, 0
        for v, x in cnt.items():
            if mx < x:
                mx = x
                ans = v
            elif mx == x:
                ans = max(ans, v)
        return ans
```

#### Java

```java
class Solution {
    public int mostFrequentPrime(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int a = -1; a <= 1; ++a) {
                    for (int b = -1; b <= 1; ++b) {
                        if (a == 0 && b == 0) {
                            continue;
                        }
                        int x = i + a, y = j + b, v = mat[i][j];
                        while (x >= 0 && x < m && y >= 0 && y < n) {
                            v = v * 10 + mat[x][y];
                            if (isPrime(v)) {
                                cnt.merge(v, 1, Integer::sum);
                            }
                            x += a;
                            y += b;
                        }
                    }
                }
            }
        }
        int ans = -1, mx = 0;
        for (var e : cnt.entrySet()) {
            int v = e.getKey(), x = e.getValue();
            if (mx < x || (mx == x && ans < v)) {
                mx = x;
                ans = v;
            }
        }
        return ans;
    }

    private boolean isPrime(int n) {
        for (int i = 2; i <= n / i; ++i) {
            if (n % i == 0) {
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
    int mostFrequentPrime(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        unordered_map<int, int> cnt;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int a = -1; a <= 1; ++a) {
                    for (int b = -1; b <= 1; ++b) {
                        if (a == 0 && b == 0) {
                            continue;
                        }
                        int x = i + a, y = j + b, v = mat[i][j];
                        while (x >= 0 && x < m && y >= 0 && y < n) {
                            v = v * 10 + mat[x][y];
                            if (isPrime(v)) {
                                cnt[v]++;
                            }
                            x += a;
                            y += b;
                        }
                    }
                }
            }
        }
        int ans = -1, mx = 0;
        for (auto& [v, x] : cnt) {
            if (mx < x || (mx == x && ans < v)) {
                mx = x;
                ans = v;
            }
        }
        return ans;
    }

private:
    bool isPrime(int n) {
        for (int i = 2; i <= n / i; ++i) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func mostFrequentPrime(mat [][]int) int {
	m, n := len(mat), len(mat[0])
	cnt := make(map[int]int)
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for a := -1; a <= 1; a++ {
				for b := -1; b <= 1; b++ {
					if a == 0 && b == 0 {
						continue
					}
					x, y, v := i+a, j+b, mat[i][j]
					for x >= 0 && x < m && y >= 0 && y < n {
						v = v*10 + mat[x][y]
						if isPrime(v) {
							cnt[v]++
						}
						x += a
						y += b
					}
				}
			}
		}
	}
	ans, mx := -1, 0
	for v, x := range cnt {
		if mx < x || (mx == x && ans < v) {
			mx = x
			ans = v
		}
	}
	return ans
}

func isPrime(n int) bool {
	for i := 2; i <= n/i; i++ {
		if n%i == 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function mostFrequentPrime(mat: number[][]): number {
    const m: number = mat.length;
    const n: number = mat[0].length;
    const cnt: Map<number, number> = new Map();
    const isPrime = (x: number): boolean => {
        for (let i = 2; i <= x / i; ++i) {
            if (x % i === 0) {
                return false;
            }
        }
        return true;
    };

    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let a = -1; a <= 1; ++a) {
                for (let b = -1; b <= 1; ++b) {
                    if (a === 0 && b === 0) {
                        continue;
                    }
                    let [x, y, v] = [i + a, j + b, mat[i][j]];
                    while (x >= 0 && x < m && y >= 0 && y < n) {
                        v = v * 10 + mat[x][y];
                        if (isPrime(v)) {
                            cnt.set(v, (cnt.get(v) || 0) + 1);
                        }
                        x += a;
                        y += b;
                    }
                }
            }
        }
    }

    let [ans, mx] = [-1, 0];
    cnt.forEach((x, v) => {
        if (mx < x || (mx === x && ans < v)) {
            mx = x;
            ans = v;
        }
    });
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
