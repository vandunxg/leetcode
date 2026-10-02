---
comments: true
difficulty: Hard
rating: 1912
source: Weekly Contest 148 Q4
tags:
    - Greedy
    - Two Pointers
    - String
    - Dynamic Programming
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [1147. Longest Chunked Palindrome Decomposition](https://leetcode.com/problems/longest-chunked-palindrome-decomposition)

[中文文档](/solution/1100-1199/1147.Longest%20Chunked%20Palindrome%20Decomposition/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>text</code>. Hãy chia chuỗi thành k chuỗi con <code>(subtext<sub>1</sub>, subtext<sub>2</sub>, ..., subtext<sub>k</sub>)</code> sao cho:</p>

<ul>
	<li><code>subtext<sub>i</sub></code> là chuỗi <strong>không rỗng</strong>.</li>
	<li>Nối tất cả các chuỗi con lại sẽ thu được <code>text</code> (tức là <code>subtext<sub>1</sub> + subtext<sub>2</sub> + ... + subtext<sub>k</sub> == text</code>).</li>
	<li><code>subtext<sub>i</sub> == subtext<sub>k - i + 1</sub></code> với mọi giá trị <code>i</code> hợp lệ (tức là <code>1 &lt;= i &lt;= k</code>).</li>
</ul>

<p>Trả về giá trị lớn nhất có thể của <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;ghiabcdefhelloadamhelloabcdefghi&quot;
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Ta có thể chia chuỗi thành các phần &quot;(ghi)(abcdef)(hello)(adam)(hello)(abcdef)(ghi)&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;merchant&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chia chuỗi thành phần &quot;(merchant)&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> text = &quot;antaprezatepzapreanta&quot;
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Ta có thể chia chuỗi thành các phần &quot;(a)(nt)(a)(pre)(za)(tep)(za)(pre)(a)(nt)(a)&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 1000</code></li>
	<li><code>text</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Một palindrome phân đoạn cần có các chunk ở hai đầu bằng nhau. Để có nhiều chunk nhất, mỗi lần ta cắt cặp prefix/suffix trùng khớp ngắn nhất; một cặp dài hơn có thể tiếp tục được chia nhỏ. Dùng hai con trỏ, tăng $k$ cho đến khi `text[i:i+k]` khớp với đoạn ở cuối chuỗi, rồi dịch hai đầu vào trong $k$ vị trí và cộng $2$ vào kết quả; nếu còn lại phần giữa thì cộng $1$.

<!-- thinking:end -->

Ta có thể bắt đầu từ hai đầu chuỗi, tìm prefix và suffix ngắn nhất sao cho chúng giống nhau và không chồng lấn:

- Nếu không tìm được prefix và suffix như vậy, ta xem toàn bộ chuỗi là một palindrome phân đoạn và tăng đáp án thêm $1$;
- Nếu tìm được prefix và suffix như vậy, ta xem cặp này là hai phần của palindrome phân đoạn, tăng đáp án thêm $2$, rồi tiếp tục tìm prefix và suffix trong phần chuỗi còn lại.

Có thể chứng minh chiến lược greedy trên như sau:

Giả sử có một prefix $A_1$ và suffix $A_2$ thỏa điều kiện, đồng thời có prefix $B_1$ và suffix $B_4$ cũng thỏa điều kiện. Vì $A_1 = A_2$ và $B_1=B_4$, nên $B_3=B_1=B_4=B_2$, đồng thời $C_1 = C_2$. Do đó, nếu greedy tách $B_1$ và $B_4$, thì phần còn lại $C_1$ và $C_2$, cũng như $B_2$ và $B_3$, vẫn có thể tách thành công. Vì vậy, ta nên chọn cặp prefix và suffix giống nhau ngắn nhất để tách trước, nhờ đó phần chuỗi còn lại có thể chia được thành nhiều palindrome phân đoạn hơn.

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1147.Longest%20Chunked%20Palindrome%20Decomposition/images/demo.png" style="width: 300px;" /></p>

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$ hoặc $O(1)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestDecomposition(self, text: str) -> int:
        ans = 0
        i, j = 0, len(text) - 1
        while i <= j:
            k = 1
            ok = False
            while i + k - 1 < j - k + 1:
                if text[i : i + k] == text[j - k + 1 : j + 1]:
                    ans += 2
                    i += k
                    j -= k
                    ok = True
                    break
                k += 1
            if not ok:
                ans += 1
                break
        return ans
```

#### Java

```java
class Solution {
    public int longestDecomposition(String text) {
        int ans = 0;
        for (int i = 0, j = text.length() - 1; i <= j;) {
            boolean ok = false;
            for (int k = 1; i + k - 1 < j - k + 1; ++k) {
                if (check(text, i, j - k + 1, k)) {
                    ans += 2;
                    i += k;
                    j -= k;
                    ok = true;
                    break;
                }
            }
            if (!ok) {
                ++ans;
                break;
            }
        }
        return ans;
    }

    private boolean check(String s, int i, int j, int k) {
        while (k-- > 0) {
            if (s.charAt(i++) != s.charAt(j++)) {
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
    int longestDecomposition(string text) {
        int ans = 0;
        auto check = [&](int i, int j, int k) -> bool {
            while (k--) {
                if (text[i++] != text[j++]) {
                    return false;
                }
            }
            return true;
        };
        for (int i = 0, j = text.size() - 1; i <= j;) {
            bool ok = false;
            for (int k = 1; i + k - 1 < j - k + 1; ++k) {
                if (check(i, j - k + 1, k)) {
                    ans += 2;
                    i += k;
                    j -= k;
                    ok = true;
                    break;
                }
            }
            if (!ok) {
                ans += 1;
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestDecomposition(text string) (ans int) {
	for i, j := 0, len(text)-1; i <= j; {
		ok := false
		for k := 1; i+k-1 < j-k+1; k++ {
			if text[i:i+k] == text[j-k+1:j+1] {
				ans += 2
				i += k
				j -= k
				ok = true
				break
			}
		}
		if !ok {
			ans++
			break
		}
	}
	return
}
```

#### TypeScript

```ts
function longestDecomposition(text: string): number {
    let ans = 0;
    for (let i = 0, j = text.length - 1; i <= j;) {
        let ok = false;
        for (let k = 1; i + k - 1 < j - k + 1; ++k) {
            if (text.slice(i, i + k) === text.slice(j - k + 1, j + 1)) {
                ans += 2;
                i += k;
                j -= k;
                ok = true;
                break;
            }
        }
        if (!ok) {
            ++ans;
            break;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: String Hash

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 so sánh các lát cắt, có thể mất $O(n^2)$. Prefix hash cho phép kiểm tra hai chuỗi con cùng độ dài trong $O(1)$. Các điểm cắt greedy vẫn giữ nguyên; chỉ cách kiểm tra hai đoạn bằng nhau thay đổi.

<!-- thinking:end -->

**String hash** ánh xạ một chuỗi có độ dài bất kỳ thành một số nguyên không âm, với xác suất collision gần bằng $0$. String hash được dùng để tính hash value của chuỗi và nhanh chóng xác định hai chuỗi có bằng nhau hay không.

Vì vậy, dựa trên Lời giải 1, ta có thể dùng string hash để so sánh hai chuỗi trong thời gian $O(1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestDecomposition(self, text: str) -> int:
        def get(l, r):
            return (h[r] - h[l - 1] * p[r - l + 1]) % mod

        n = len(text)
        base = 131
        mod = int(1e9) + 7
        h = [0] * (n + 10)
        p = [1] * (n + 10)
        for i, c in enumerate(text):
            t = ord(c) - ord('a') + 1
            h[i + 1] = (h[i] * base) % mod + t
            p[i + 1] = (p[i] * base) % mod

        ans = 0
        i, j = 0, n - 1
        while i <= j:
            k = 1
            ok = False
            while i + k - 1 < j - k + 1:
                if get(i + 1, i + k) == get(j - k + 2, j + 1):
                    ans += 2
                    i += k
                    j -= k
                    ok = True
                    break
                k += 1
            if not ok:
                ans += 1
                break
        return ans
```

#### Java

```java
class Solution {
    private long[] h;
    private long[] p;

    public int longestDecomposition(String text) {
        int n = text.length();
        int base = 131;
        h = new long[n + 10];
        p = new long[n + 10];
        p[0] = 1;
        for (int i = 0; i < n; ++i) {
            int t = text.charAt(i) - 'a' + 1;
            h[i + 1] = h[i] * base + t;
            p[i + 1] = p[i] * base;
        }
        int ans = 0;
        for (int i = 0, j = n - 1; i <= j;) {
            boolean ok = false;
            for (int k = 1; i + k - 1 < j - k + 1; ++k) {
                if (get(i + 1, i + k) == get(j - k + 2, j + 1)) {
                    ans += 2;
                    i += k;
                    j -= k;
                    ok = true;
                    break;
                }
            }
            if (!ok) {
                ++ans;
                break;
            }
        }
        return ans;
    }

    private long get(int i, int j) {
        return h[j] - h[i - 1] * p[j - i + 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestDecomposition(string text) {
        using ull = unsigned long long;
        int n = text.size();
        int base = 131;
        ull p[n + 10];
        ull h[n + 10];
        p[0] = 1;
        h[0] = 0;
        for (int i = 0; i < n; ++i) {
            int t = text[i] - 'a' + 1;
            p[i + 1] = p[i] * base;
            h[i + 1] = h[i] * base + t;
        }

        int ans = 0;
        auto get = [&](int l, int r) {
            return h[r] - h[l - 1] * p[r - l + 1];
        };
        for (int i = 0, j = n - 1; i <= j;) {
            bool ok = false;
            for (int k = 1; i + k - 1 < j - k + 1; ++k) {
                if (get(i + 1, i + k) == get(j - k + 2, j + 1)) {
                    ans += 2;
                    i += k;
                    j -= k;
                    ok = true;
                    break;
                }
            }
            if (!ok) {
                ++ans;
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestDecomposition(text string) (ans int) {
	n := len(text)
	base := 131
	h := make([]int, n+10)
	p := make([]int, n+10)
	p[0] = 1
	for i, c := range text {
		t := int(c-'a') + 1
		p[i+1] = p[i] * base
		h[i+1] = h[i]*base + t
	}
	get := func(l, r int) int {
		return h[r] - h[l-1]*p[r-l+1]
	}

	for i, j := 0, n-1; i <= j; {
		ok := false
		for k := 1; i+k-1 < j-k+1; k++ {
			if get(i+1, i+k) == get(j-k+2, j+1) {
				ans += 2
				i += k
				j -= k
				ok = true
				break
			}
		}
		if !ok {
			ans++
			break
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
