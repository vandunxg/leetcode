---
comments: true
difficulty: Medium
rating: 1636
source: Weekly Contest 307 Q2
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2384. Largest Palindromic Number](https://leetcode.com/problems/largest-palindromic-number)

[中文文档](/solution/2300-2399/2384.Largest%20Palindromic%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>num</code> chỉ gồm các chữ số.</p>

<p>Trả về <em>số nguyên <strong>đối xứng lớn nhất</strong> (dưới dạng chuỗi) có thể được tạo thành bằng các chữ số lấy từ </em><code>num</code>. Số đó không được chứa <strong>các số 0 ở đầu</strong>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Bạn <strong>không cần</strong> sử dụng tất cả các chữ số của <code>num</code>, nhưng phải sử dụng <strong>ít nhất</strong> một chữ số.</li>
	<li>Các chữ số có thể được sắp xếp lại.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;444947137&quot;
<strong>Đầu ra:</strong> &quot;7449447&quot;
<strong>Giải thích:</strong>
Sử dụng các chữ số &quot;4449477&quot; từ &quot;<u><strong>44494</strong></u><u><strong>7</strong></u>13<u><strong>7</strong></u>&quot; để tạo thành số nguyên đối xứng &quot;7449447&quot;.
Có thể chứng minh rằng &quot;7449447&quot; là số nguyên đối xứng lớn nhất có thể tạo thành.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;00009&quot;
<strong>Đầu ra:</strong> &quot;9&quot;
<strong>Giải thích:</strong>
Có thể chứng minh rằng &quot;9&quot; là số nguyên đối xứng lớn nhất có thể tạo thành.
Lưu ý rằng số nguyên được trả về không được chứa các số 0 ở đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại các chữ số để tạo số đối xứng lớn nhất, bỏ bớt nếu cần. Vì $n \le 10^5$, ta tham lam trên số lần xuất hiện của từng chữ số thay vì xét các hoán vị.
>
> Sau khi đếm, chữ số lớn nhất có số lần xuất hiện lẻ được chọn làm chữ số ở giữa (sau đó số lần xuất hiện của nó là chẵn). Lấy một nửa số lần xuất hiện còn lại của mỗi chữ số, rồi đối xứng từ $0$ đến $9$. Loại bỏ các số 0; nếu không còn gì thì đáp án là $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestPalindromic(self, num: str) -> str:
        cnt = Counter(num)
        ans = ''
        for i in range(9, -1, -1):
            v = str(i)
            if cnt[v] % 2:
                ans = v
                cnt[v] -= 1
                break
        for i in range(10):
            v = str(i)
            if cnt[v]:
                cnt[v] //= 2
                s = cnt[v] * v
                ans = s + ans + s
        return ans.strip('0') or '0'
```

#### Java

```java
class Solution {
    public String largestPalindromic(String num) {
        int[] cnt = new int[10];
        for (char c : num.toCharArray()) {
            ++cnt[c - '0'];
        }
        String mid = "";
        for (int i = 9; i >= 0; --i) {
            if (cnt[i] % 2 == 1) {
                mid += i;
                --cnt[i];
                break;
            }
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 10; ++i) {
            if (cnt[i] > 0) {
                cnt[i] >>= 1;
                sb.append(("" + i).repeat(cnt[i]));
            }
        }
        while (sb.length() > 0 && sb.charAt(sb.length() - 1) == '0') {
            sb.deleteCharAt(sb.length() - 1);
        }
        String t = sb.toString();
        String ans = sb.reverse().toString() + mid + t;
        return "".equals(ans) ? "0" : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestPalindromic(string num) {
        vector<int> cnt(10);
        for (char c : num) ++cnt[c - '0'];
        string mid = "";
        for (int i = 9; ~i; --i) {
            if (cnt[i] % 2) {
                mid += (i + '0');
                --cnt[i];
                break;
            }
        }
        string t = "";
        for (int i = 0; i < 10; ++i) {
            if (cnt[i]) {
                cnt[i] >>= 1;
                while (cnt[i]--) {
                    t += (i + '0');
                }
            }
        }
        while (t.size() && t.back() == '0') {
            t.pop_back();
        }
        string ans = t;
        reverse(ans.begin(), ans.end());
        ans += mid + t;
        return ans == "" ? "0" : ans;
    }
};
```

#### Go

```go
func largestPalindromic(num string) string {
	cnt := make([]int, 10)
	for _, c := range num {
		cnt[c-'0']++
	}
	ans := ""
	for i := 9; i >= 0; i-- {
		if cnt[i]%2 == 1 {
			ans = strconv.Itoa(i)
			cnt[i]--
			break
		}
	}
	for i := 0; i < 10; i++ {
		if cnt[i] > 0 {
			cnt[i] >>= 1
			s := strings.Repeat(strconv.Itoa(i), cnt[i])
			ans = s + ans + s
		}
	}
	ans = strings.Trim(ans, "0")
	if ans == "" {
		return "0"
	}
	return ans
}
```

#### TypeScript

```ts
function largestPalindromic(num: string): string {
    const count = new Array(10).fill(0);
    for (const c of num) {
        count[c]++;
    }
    while (count.reduce((r, v) => (v % 2 === 1 ? r + 1 : r), 0) > 1) {
        for (let i = 0; i < 10; i++) {
            if (count[i] % 2 === 1) {
                count[i]--;
                break;
            }
        }
    }

    let res = [];
    let oddIndex = -1;
    for (let i = 9; i >= 0; i--) {
        if (count[i] % 2 == 1) {
            oddIndex = i;
            count[i] -= 1;
        }
        res.push(...new Array(count[i] >> 1).fill(i));
    }
    if (oddIndex !== -1) {
        res.push(oddIndex);
    }
    const n = res.length;
    for (let i = 0; i < n; i++) {
        if (res[i] !== 0) {
            res = res.slice(i);
            if (oddIndex !== -1) {
                res.push(...[...res.slice(0, res.length - 1)].reverse());
            } else {
                res.push(...[...res].reverse());
            }
            return res.join('');
        }
    }

    return '0';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
