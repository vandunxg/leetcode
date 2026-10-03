---
comments: true
difficulty: Hard
rating: 2209
source: Weekly Contest 268 Q4
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [2081. Sum of k-Mirror Numbers](https://leetcode.com/problems/sum-of-k-mirror-numbers)

[中文文档](/solution/2000-2099/2081.Sum%20of%20k-Mirror%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>số k-mirror</strong> là một số nguyên <strong>dương</strong>, <strong>không có các chữ số 0 ở đầu</strong>, đọc xuôi và đọc ngược giống nhau trong cả hệ cơ số 10 <strong>lẫn</strong> hệ cơ số k.</p>

<ul>
	<li>Ví dụ, <code>9</code> là một số 2-mirror. Biểu diễn của <code>9</code> trong hệ cơ số 10 và cơ số 2 lần lượt là <code>9</code> và <code>1001</code>, đều đọc xuôi và đọc ngược giống nhau.</li>
	<li>Ngược lại, <code>4</code> không phải là số 2-mirror. Biểu diễn của <code>4</code> trong hệ cơ số 2 là <code>100</code>, không đọc xuôi và đọc ngược giống nhau.</li>
</ul>

<p>Cho hai số <code>k</code> và <code>n</code>, hãy trả về <em><strong>tổng</strong> của</em> <code>n</code> <em><strong>số k-mirror nhỏ nhất</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 2, n = 5
<strong>Đầu ra:</strong> 25
<strong>Giải thích:
</strong>5 số 2-mirror nhỏ nhất và biểu diễn của chúng trong hệ cơ số 2 được liệt kê như sau:
  base-10    base-2
    1          1
    3          11
    5          101
    7          111
    9          1001
Tổng của chúng = 1 + 3 + 5 + 7 + 9 = 25.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3, n = 7
<strong>Đầu ra:</strong> 499
<strong>Giải thích:
</strong>7 số 3-mirror nhỏ nhất và biểu diễn của chúng trong hệ cơ số 3 được liệt kê như sau:
  base-10    base-3
    1          1
    2          2
    4          11
    8          22
    121        11111
    151        12121
    212        21212
Tổng của chúng = 1 + 2 + 4 + 8 + 121 + 151 + 212 = 499.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 7, n = 17
<strong>Đầu ra:</strong> 20379000
<strong>Giải thích:</strong> 17 số 7-mirror nhỏ nhất là:
1, 2, 3, 4, 5, 6, 8, 121, 171, 242, 292, 16561, 65656, 2137312, 4602064, 6597956, 6958596
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= k &lt;= 9</code></li>
	<li><code>1 &lt;= n &lt;= 30</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một nửa + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 30$ nhưng các số $k$-mirror khá thưa, nên duyệt lần lượt các số nguyên là lãng phí. Một palindrome trong hệ thập phân được xác định bởi nửa đầu của nó, và phần này có phạm vi cần duyệt nhỏ hơn nhiều.
>
> Với mỗi độ dài, đối xứng hóa nửa đầu, sau đó kiểm tra các chữ số trong hệ cơ số $k$. Tính tổng $n$ số đầu tiên thỏa mãn.

<!-- thinking:end -->

Với một số k-mirror, ta có thể chia nó thành hai phần: nửa đầu và nửa sau. Với các số có độ dài chẵn, nửa đầu và nửa sau hoàn toàn giống nhau; với các số có độ dài lẻ, nửa đầu và nửa sau giống nhau, nhưng chữ số ở giữa có thể là bất kỳ chữ số nào.

Ta có thể duyệt các số ở nửa đầu, sau đó xây dựng số k-mirror hoàn chỉnh dựa trên nửa đầu. Các bước cụ thể như sau:

1. **Duyệt các độ dài**: Bắt đầu duyệt độ dài của các số từ 1, cho đến khi tìm đủ số k-mirror thỏa mãn yêu cầu.
2. **Tính phạm vi của nửa đầu**: Với một số có độ dài $l$, phạm vi của nửa đầu là $[10^{(l-1)/2}, 10^{(l+1)/2})$.
3. **Xây dựng số k-mirror**: Với mỗi số $i$ trong nửa đầu, nếu độ dài là chẵn, dùng trực tiếp $i$ làm nửa đầu; nếu độ dài là lẻ, chia $i$ cho 10 để lấy nửa đầu. Sau đó đảo các chữ số của nửa đầu và nối chúng vào để tạo thành số k-mirror hoàn chỉnh.
4. **Kiểm tra số k-mirror**: Chuyển số vừa xây dựng sang hệ cơ số $k$ và kiểm tra xem nó có phải là palindrome hay không.
5. **Cộng dồn kết quả**: Nếu đó là một số k-mirror, cộng nó vào kết quả và giảm bộ đếm $n$. Khi $n$ bằng 0, trả về kết quả.

Độ phức tạp thời gian chủ yếu phụ thuộc vào độ dài được duyệt và phạm vi của nửa đầu. Vì giá trị lớn nhất của $n$ là 30, số lần duyệt trên thực tế bị giới hạn. Độ phức tạp không gian là $O(1)$, vì chỉ sử dụng một lượng bộ nhớ bổ sung không đổi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kMirror(self, k: int, n: int) -> int:
        def check(x: int, k: int) -> bool:
            s = []
            while x:
                s.append(x % k)
                x //= k
            return s == s[::-1]

        ans = 0
        for l in count(1):
            x = 10 ** ((l - 1) // 2)
            y = 10 ** ((l + 1) // 2)
            for i in range(x, y):
                v = i
                j = i if l % 2 == 0 else i // 10
                while j > 0:
                    v = v * 10 + j % 10
                    j //= 10
                if check(v, k):
                    ans += v
                    n -= 1
                    if n == 0:
                        return ans
```

#### Java

```java
class Solution {
    public long kMirror(int k, int n) {
        long ans = 0;
        for (int l = 1;; l++) {
            int x = (int) Math.pow(10, (l - 1) / 2);
            int y = (int) Math.pow(10, (l + 1) / 2);
            for (int i = x; i < y; i++) {
                long v = i;
                int j = (l % 2 == 0) ? i : i / 10;
                while (j > 0) {
                    v = v * 10 + j % 10;
                    j /= 10;
                }
                if (check(v, k)) {
                    ans += v;
                    n--;
                    if (n == 0) {
                        return ans;
                    }
                }
            }
        }
    }

    private boolean check(long x, int k) {
        List<Integer> s = new ArrayList<>();
        while (x > 0) {
            s.add((int) (x % k));
            x /= k;
        }
        for (int i = 0, j = s.size() - 1; i < j; ++i, --j) {
            if (!s.get(i).equals(s.get(j))) {
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
    long long kMirror(int k, int n) {
        long long ans = 0;
        for (int l = 1;; ++l) {
            int x = pow(10, (l - 1) / 2);
            int y = pow(10, (l + 1) / 2);
            for (int i = x; i < y; ++i) {
                long long v = i;
                int j = (l % 2 == 0) ? i : i / 10;
                while (j > 0) {
                    v = v * 10 + j % 10;
                    j /= 10;
                }
                if (check(v, k)) {
                    ans += v;
                    if (--n == 0) {
                        return ans;
                    }
                }
            }
        }
    }

private:
    bool check(long long x, int k) {
        vector<int> s;
        while (x > 0) {
            s.push_back(x % k);
            x /= k;
        }
        for (int i = 0, j = s.size() - 1; i < j; ++i, --j) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func kMirror(k int, n int) int64 {
	check := func(x int64, k int) bool {
		s := []int{}
		for x > 0 {
			s = append(s, int(x%int64(k)))
			x /= int64(k)
		}
		for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
			if s[i] != s[j] {
				return false
			}
		}
		return true
	}

	var ans int64 = 0
	for l := 1; ; l++ {
		x := pow10((l - 1) / 2)
		y := pow10((l + 1) / 2)
		for i := x; i < y; i++ {
			v := int64(i)
			j := i
			if l%2 != 0 {
				j = i / 10
			}
			for j > 0 {
				v = v*10 + int64(j%10)
				j /= 10
			}
			if check(v, k) {
				ans += v
				n--
				if n == 0 {
					return ans
				}
			}
		}
	}
}

func pow10(exp int) int {
	res := 1
	for i := 0; i < exp; i++ {
		res *= 10
	}
	return res
}
```

#### TypeScript

```ts
function kMirror(k: number, n: number): number {
    function check(x: number, k: number): boolean {
        const s: number[] = [];
        while (x > 0) {
            s.push(x % k);
            x = Math.floor(x / k);
        }
        for (let i = 0, j = s.length - 1; i < j; i++, j--) {
            if (s[i] !== s[j]) {
                return false;
            }
        }
        return true;
    }

    let ans = 0;
    for (let l = 1; ; l++) {
        const x = Math.pow(10, Math.floor((l - 1) / 2));
        const y = Math.pow(10, Math.floor((l + 1) / 2));
        for (let i = x; i < y; i++) {
            let v = i;
            let j = l % 2 === 0 ? i : Math.floor(i / 10);
            while (j > 0) {
                v = v * 10 + (j % 10);
                j = Math.floor(j / 10);
            }
            if (check(v, k)) {
                ans += v;
                n--;
                if (n === 0) {
                    return ans;
                }
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
