---
comments: true
difficulty: Medium
rating: 1822
source: Weekly Contest 286 Q3
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [2217. Find Palindrome With Fixed Length](https://leetcode.com/problems/find-palindrome-with-fixed-length)

[中文文档](/solution/2200-2299/2217.Find%20Palindrome%20With%20Fixed%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>queries</code> và một số nguyên <strong>dương</strong> <code>intLength</code>, hãy trả về <em>một mảng</em> <code>answer</code> <em>trong đó</em> <code>answer[i]</code> <em>là </em><code>queries[i]<sup>th</sup></code> <em>số <strong>đối xứng dương</strong> nhỏ nhất có độ dài</em> <code>intLength</code> <em>hoặc</em> <code>-1</code><em> nếu không tồn tại số đối xứng như vậy</em>.</p>

<p><strong>Số đối xứng</strong> là một số đọc xuôi hay ngược đều giống nhau. Số đối xứng không được có số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [1,2,3,4,5,90], intLength = 3
<strong>Đầu ra:</strong> [101,111,121,131,141,999]
<strong>Giải thích:</strong>
Một vài số đối xứng đầu tiên có độ dài 3 là:
101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, ...
Số đối xứng thứ 90<sup>th</sup> có độ dài 3 là 999.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> queries = [2,4,6], intLength = 4
<strong>Đầu ra:</strong> [1111,1331,1551]
<strong>Giải thích:</strong>
Sáu số đối xứng đầu tiên có độ dài 4 là:
1001, 1111, 1221, 1331, 1441 và 1551.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= queries[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= intLength&nbsp;&lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số đối xứng thứ $q$ có độ dài $\textit{intLength}$. $q$ có thể lên tới $10^9$ và độ dài có thể lên tới $15$, nên không thể liệt kê các số đối xứng. Một số đối xứng được xác định bởi nửa đầu của nó; nửa sau là phần đối xứng qua gương.
>
> Nửa đầu có độ dài $\lceil \textit{intLength}/2 \rceil$ và nằm trong khoảng từ $10^{l-1}$ đến $10^l-1$. Truy vấn $q$ tương ứng với $v = 10^{l-1}+q-1$; nếu $v$ vượt quá giới hạn thì đáp án là $-1$, ngược lại đối xứng các chữ số của $v$ (bỏ bản sao ở giữa khi độ dài là số lẻ).

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthPalindrome(self, queries: List[int], intLength: int) -> List[int]:
        l = (intLength + 1) >> 1
        start, end = 10 ** (l - 1), 10**l - 1
        ans = []
        for q in queries:
            v = start + q - 1
            if v > end:
                ans.append(-1)
                continue
            s = str(v)
            s += s[::-1][intLength % 2 :]
            ans.append(int(s))
        return ans
```

#### Java

```java
class Solution {
    public long[] kthPalindrome(int[] queries, int intLength) {
        int n = queries.length;
        long[] ans = new long[n];
        int l = (intLength + 1) >> 1;
        long start = (long) Math.pow(10, l - 1);
        long end = (long) Math.pow(10, l) - 1;
        for (int i = 0; i < n; ++i) {
            long v = start + queries[i] - 1;
            if (v > end) {
                ans[i] = -1;
                continue;
            }
            String s = "" + v;
            s += new StringBuilder(s).reverse().substring(intLength % 2);
            ans[i] = Long.parseLong(s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> kthPalindrome(vector<int>& queries, int intLength) {
        int l = (intLength + 1) >> 1;
        long long start = pow(10, l - 1), end = pow(10, l) - 1;
        vector<long long> ans;
        for (int& q : queries) {
            long long v = start + q - 1;
            if (v > end) {
                ans.push_back(-1);
                continue;
            }
            string s = to_string(v);
            string s1 = s;
            reverse(s1.begin(), s1.end());
            s += s1.substr(intLength % 2);
            ans.push_back(stoll(s));
        }
        return ans;
    }
};
```

#### Go

```go
func kthPalindrome(queries []int, intLength int) []int64 {
	l := (intLength + 1) >> 1
	start, end := int(math.Pow10(l-1)), int(math.Pow10(l))-1
	var ans []int64
	for _, q := range queries {
		v := start + q - 1
		if v > end {
			ans = append(ans, -1)
			continue
		}
		t := v
		if intLength%2 == 1 {
			t /= 10
		}
		for t > 0 {
			v = v*10 + t%10
			t /= 10
		}
		ans = append(ans, int64(v))
	}
	return ans
}
```

#### TypeScript

```ts
function kthPalindrome(queries: number[], intLength: number): number[] {
    const isOdd = intLength % 2 === 1;
    const bestNum = 10 ** ((intLength >> 1) + (isOdd ? 1 : 0) - 1);
    const max = bestNum * 9;
    return queries.map(v => {
        if (v > max) {
            return -1;
        }
        const num = bestNum + v - 1;
        return Number(
            num +
                (num + '')
                    .split('')
                    .reverse()
                    .slice(isOdd ? 1 : 0)
                    .join(''),
        );
    });
}
```

#### Rust

```rust
impl Solution {
    pub fn kth_palindrome(queries: Vec<i32>, int_length: i32) -> Vec<i64> {
        let is_odd = (int_length & 1) == 1;
        let best_num = i32::pow(10, (int_length / 2 + (if is_odd { 0 } else { -1 })) as u32);
        let max = best_num * 9;
        queries
            .iter()
            .map(|&num| {
                if num > max {
                    return -1;
                }
                let num = best_num + num - 1;
                format!(
                    "{}{}",
                    num,
                    num.to_string()
                        .chars()
                        .rev()
                        .skip(if is_odd { 1 } else { 0 })
                        .collect::<String>()
                )
                .parse()
                .unwrap()
            })
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
