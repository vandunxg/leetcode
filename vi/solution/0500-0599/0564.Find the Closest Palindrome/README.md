---
comments: true
difficulty: Hard
tags:
    - Math
    - String
---

<!-- problem:start -->

# [564. Find the Closest Palindrome](https://leetcode.com/problems/find-the-closest-palindrome)

[中文文档](/solution/0500-0599/0564.Find%20the%20Closest%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>n</code> biểu diễn một số nguyên, hãy trả về <em>số nguyên đối xứng gần nhất (không tính chính nó)</em>. Nếu có nhiều đáp án cùng khoảng cách, hãy trả về <em><strong>số nhỏ hơn</strong></em>.</p>

<p>Gần nhất nghĩa là có hiệu tuyệt đối nhỏ nhất giữa hai số nguyên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;123&quot;
<strong>Đầu ra:</strong> &quot;121&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;1&quot;
<strong>Đầu ra:</strong> &quot;0&quot;
<strong>Giải thích:</strong> 0 và 2 là hai số đối xứng gần nhất, nhưng ta trả về số nhỏ hơn là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n.length &lt;= 18</code></li>
	<li><code>n</code> chỉ gồm các chữ số.</li>
	<li><code>n</code> không có số 0 ở đầu.</li>
	<li><code>n</code> biểu diễn số nguyên trong khoảng <code>[1, 10<sup>18</sup> - 1]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số đối xứng gần nhất nhưng không bằng $n$. Duyệt các số nguyên lân cận không có giới hạn rõ ràng về số lượng cần xét.
>
> Các số đối xứng gần nhất có thể được tạo bằng cách đối xứng prefix, prefix đó cộng hoặc trừ $1$, cùng hai trường hợp đổi độ dài là $99\ldots9$ và $100\ldots001$. Tạo các số đối xứng ứng viên, loại chính $n$, rồi chọn theo khoảng cách và sau đó theo giá trị. Chỉ có một số lượng ứng viên hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nearestPalindromic(self, n: str) -> str:
        x = int(n)
        l = len(n)
        res = {10 ** (l - 1) - 1, 10**l + 1}
        left = int(n[: (l + 1) >> 1])
        for i in range(left - 1, left + 2):
            j = i if l % 2 == 0 else i // 10
            while j:
                i = i * 10 + j % 10
                j //= 10
            res.add(i)
        res.discard(x)

        ans = -1
        for t in res:
            if (
                ans == -1
                or abs(t - x) < abs(ans - x)
                or (abs(t - x) == abs(ans - x) and t < ans)
            ):
                ans = t
        return str(ans)
```

#### Java

```java
class Solution {
    public String nearestPalindromic(String n) {
        long x = Long.parseLong(n);
        long ans = -1;
        for (long t : get(n)) {
            if (ans == -1 || Math.abs(t - x) < Math.abs(ans - x)
                || (Math.abs(t - x) == Math.abs(ans - x) && t < ans)) {
                ans = t;
            }
        }
        return Long.toString(ans);
    }

    private Set<Long> get(String n) {
        int l = n.length();
        Set<Long> res = new HashSet<>();
        res.add((long) Math.pow(10, l - 1) - 1);
        res.add((long) Math.pow(10, l) + 1);
        long left = Long.parseLong(n.substring(0, (l + 1) / 2));
        for (long i = left - 1; i <= left + 1; ++i) {
            StringBuilder sb = new StringBuilder();
            sb.append(i);
            sb.append(new StringBuilder(i + "").reverse().substring(l & 1));
            res.add(Long.parseLong(sb.toString()));
        }
        res.remove(Long.parseLong(n));
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string nearestPalindromic(string n) {
        long x = stol(n);
        long ans = -1;
        for (long t : get(n))
            if (ans == -1 || abs(t - x) < abs(ans - x) || (abs(t - x) == abs(ans - x) && t < ans))
                ans = t;
        return to_string(ans);
    }

    unordered_set<long> get(string& n) {
        int l = n.size();
        unordered_set<long> res;
        res.insert((long) pow(10, l - 1) - 1);
        res.insert((long) pow(10, l) + 1);
        long left = stol(n.substr(0, (l + 1) / 2));
        for (long i = left - 1; i <= left + 1; ++i) {
            string prefix = to_string(i);
            string t = prefix + string(prefix.rbegin() + (l & 1), prefix.rend());
            res.insert(stol(t));
        }
        res.erase(stol(n));
        return res;
    }
};
```

#### Go

```go
func nearestPalindromic(n string) string {
	l := len(n)
	res := []int{int(math.Pow10(l-1)) - 1, int(math.Pow10(l)) + 1}
	left, _ := strconv.Atoi(n[:(l+1)/2])
	for _, x := range []int{left - 1, left, left + 1} {
		y := x
		if l&1 == 1 {
			y /= 10
		}
		for ; y > 0; y /= 10 {
			x = x*10 + y%10
		}
		res = append(res, x)
	}
	ans := -1
	x, _ := strconv.Atoi(n)
	for _, t := range res {
		if t != x {
			if ans == -1 || abs(t-x) < abs(ans-x) || abs(t-x) == abs(ans-x) && t < ans {
				ans = t
			}
		}
	}
	return strconv.Itoa(ans)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### JavaScript

```js
/**
 * @param {string} n
 * @return {string}
 */

function nearestPalindromic(n) {
    const x = BigInt(n);
    let ans = null;

    for (const t of getCandidates(n)) {
        if (
            ans === null ||
            absDiff(t, x) < absDiff(ans, x) ||
            (absDiff(t, x) === absDiff(ans, x) && t < ans)
        ) {
            ans = t;
        }
    }

    return ans.toString();
}

function getCandidates(n) {
    const length = n.length;
    const res = new Set();

    res.add(BigInt(Math.pow(10, length - 1) - 1));
    res.add(BigInt(Math.pow(10, length) + 1));

    const left = BigInt(n.substring(0, Math.ceil(length / 2)));

    for (let i = left - 1n; i <= left + 1n; i++) {
        const prefix = i.toString();
        const t =
            prefix +
            prefix
                .split('')
                .reverse()
                .slice(length % 2)
                .join('');
        res.add(BigInt(t));
    }

    res.delete(BigInt(n));
    return res;
}

function absDiff(a, b) {
    return a > b ? a - b : b - a;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
