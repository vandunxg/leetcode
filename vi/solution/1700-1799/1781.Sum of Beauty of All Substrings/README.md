---
comments: true
difficulty: Medium
rating: 1714
source: Biweekly Contest 47 Q3
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [1781. Sum of Beauty of All Substrings](https://leetcode.com/problems/sum-of-beauty-of-all-substrings)

[中文文档](/solution/1700-1799/1781.Sum%20of%20Beauty%20of%20All%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Vẻ đẹp</strong> của một chuỗi là chênh lệch tần suất giữa ký tự xuất hiện nhiều nhất và ký tự xuất hiện ít nhất.</p>

<ul>
	<li>Ví dụ, vẻ đẹp của <code>&quot;abaacc&quot;</code> là <code>3 - 1 = 2</code>.</li>
</ul>

<p>Cho chuỗi <code>s</code>, trả về <em>tổng <strong>vẻ đẹp</strong> của tất cả chuỗi con.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabcb&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích: </strong>Các chuỗi con có vẻ đẹp khác 0 là [&quot;aab&quot;,&quot;aabc&quot;,&quot;aabcb&quot;,&quot;abcb&quot;,&quot;bcb&quot;], mỗi chuỗi có vẻ đẹp bằng 1.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aabcbaa&quot;
<strong>Đầu ra:</strong> 17
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;=<sup> </sup>500</code></li>
	<li><code>s</code> consists of only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Vẻ đẹp là chênh lệch giữa tần suất lớn nhất và nhỏ nhất của các chữ cái trong chuỗi con. $n\le 500$ cho phép xét tất cả cửa sổ $O(n^2)$.
>
> Cố định đầu trái, mở rộng đầu phải đồng thời cập nhật bộ đếm, rồi cộng $\max-\min$ sau mỗi lần mở rộng.

<!-- thinking:end -->

Liệt kê vị trí bắt đầu $i$ của mỗi chuỗi con, tìm tất cả chuỗi con có ký tự tại vị trí này làm đầu trái, sau đó tính vẻ đẹp của từng chuỗi con và cộng dồn vào đáp án.

Độ phức tạp thời gian là $O(n^2 \times C)$, còn độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài chuỗi và $C$ là kích thước tập ký tự. Ở bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautySum(self, s: str) -> int:
        ans, n = 0, len(s)
        for i in range(n):
            cnt = Counter()
            for j in range(i, n):
                cnt[s[j]] += 1
                ans += max(cnt.values()) - min(cnt.values())
        return ans
```

#### Java

```java
class Solution {
    public int beautySum(String s) {
        int ans = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int[] cnt = new int[26];
            for (int j = i; j < n; ++j) {
                ++cnt[s.charAt(j) - 'a'];
                int mi = 1000, mx = 0;
                for (int v : cnt) {
                    if (v > 0) {
                        mi = Math.min(mi, v);
                        mx = Math.max(mx, v);
                    }
                }
                ans += mx - mi;
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
    int beautySum(string s) {
        int ans = 0;
        int n = s.size();
        int cnt[26];
        for (int i = 0; i < n; ++i) {
            memset(cnt, 0, sizeof cnt);
            for (int j = i; j < n; ++j) {
                ++cnt[s[j] - 'a'];
                int mi = 1000, mx = 0;
                for (int& v : cnt) {
                    if (v > 0) {
                        mi = min(mi, v);
                        mx = max(mx, v);
                    }
                }
                ans += mx - mi;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func beautySum(s string) (ans int) {
	for i := range s {
		cnt := [26]int{}
		for j := i; j < len(s); j++ {
			cnt[s[j]-'a']++
			mi, mx := 1000, 0
			for _, v := range cnt {
				if v > 0 {
					if mi > v {
						mi = v
					}
					if mx < v {
						mx = v
					}
				}
			}
			ans += mx - mi
		}
	}
	return
}
```

#### TypeScript

```ts
function beautySum(s: string): number {
    let ans = 0;
    for (let i = 0; i < s.length; ++i) {
        const cnt = new Map();
        for (let j = i; j < s.length; ++j) {
            cnt.set(s[j], (cnt.get(s[j]) || 0) + 1);
            const t = Array.from(cnt.values());
            ans += Math.max(...t) - Math.min(...t);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn beauty_sum(s: String) -> i32 {
        let mut ans = 0;
        let n = s.len();
        let s: Vec<char> = s.chars().collect();

        for i in 0..n {
            let mut cnt = vec![0; 26];
            for j in i..n {
                cnt[s[j] as usize - 'a' as usize] += 1;
                let mut mi = 1000;
                let mut mx = 0;
                for &v in &cnt {
                    if v > 0 {
                        mi = mi.min(v);
                        mx = mx.max(v);
                    }
                }
                ans += mx - mi;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var beautySum = function (s) {
    let ans = 0;
    for (let i = 0; i < s.length; ++i) {
        const cnt = new Map();
        for (let j = i; j < s.length; ++j) {
            cnt.set(s[j], (cnt.get(s[j]) || 0) + 1);
            const t = Array.from(cnt.values());
            ans += Math.max(...t) - Math.min(...t);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quét lại bộ đếm để tìm min và max. Theo dõi tần suất của các tần suất cùng với $mi,mx$ hiện tại giúp cập nhật hai đầu trong $O(1)$ sau mỗi lần thêm ký tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautySum(self, s: str) -> int:
        ans, n = 0, len(s)
        for i in range(n):
            cnt = Counter()
            freq = Counter()
            mi = mx = 1
            for j in range(i, n):
                freq[cnt[s[j]]] -= 1
                cnt[s[j]] += 1
                freq[cnt[s[j]]] += 1

                if cnt[s[j]] == 1:
                    mi = 1
                if freq[mi] == 0:
                    mi += 1
                if cnt[s[j]] > mx:
                    mx = cnt[s[j]]

                ans += mx - mi
        return ans
```

#### Java

```java
class Solution {
    public int beautySum(String s) {
        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int[] cnt = new int[26];
            Map<Integer, Integer> freq = new HashMap<>();
            int mi = 1, mx = 1;
            for (int j = i; j < n; ++j) {
                int k = s.charAt(j) - 'a';
                freq.merge(cnt[k], -1, Integer::sum);
                ++cnt[k];
                freq.merge(cnt[k], 1, Integer::sum);

                if (cnt[k] == 1) {
                    mi = 1;
                }
                if (freq.getOrDefault(mi, 0) == 0) {
                    ++mi;
                }
                if (cnt[k] > mx) {
                    mx = cnt[k];
                }
                ans += mx - mi;
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
    int beautySum(string s) {
        int n = s.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int cnt[26]{};
            unordered_map<int, int> freq;
            int mi = 1, mx = 1;
            for (int j = i; j < n; ++j) {
                int k = s[j] - 'a';
                --freq[cnt[k]];
                ++cnt[k];
                ++freq[cnt[k]];

                if (cnt[k] == 1) {
                    mi = 1;
                }
                if (freq[mi] == 0) {
                    ++mi;
                }
                if (cnt[k] > mx) {
                    mx = cnt[k];
                }
                ans += mx - mi;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func beautySum(s string) (ans int) {
	n := len(s)
	for i := 0; i < n; i++ {
		cnt := [26]int{}
		freq := map[int]int{}
		mi, mx := 1, 1
		for j := i; j < n; j++ {
			k := int(s[j] - 'a')
			freq[cnt[k]]--
			cnt[k]++
			freq[cnt[k]]++

			if cnt[k] == 1 {
				mi = 1
			}
			if freq[mi] == 0 {
				mi++
			}
			if cnt[k] > mx {
				mx = cnt[k]
			}
			ans += mx - mi
		}
	}
	return
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var beautySum = function (s) {
    const n = s.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const cnt = Array(26).fill(0);
        const freq = new Map();
        let [mi, mx] = [1, 1];
        for (let j = i; j < n; ++j) {
            const k = s[j].charCodeAt() - 97;
            freq.set(cnt[k], (freq.get(cnt[k]) || 0) - 1);
            ++cnt[k];
            freq.set(cnt[k], (freq.get(cnt[k]) || 0) + 1);
            if (cnt[k] === 1) {
                mi = 1;
            }
            if (freq.get(mi) === 0) {
                ++mi;
            }
            if (cnt[k] > mx) {
                mx = cnt[k];
            }
            ans += mx - mi;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
