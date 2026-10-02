---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [916. Word Subsets](https://leetcode.com/problems/word-subsets)

[中文文档](/solution/0900-0999/0916.Word%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>words1</code> và <code>words2</code>.</p>

<p>Chuỗi <code>b</code> là <strong>tập con</strong> của chuỗi <code>a</code> nếu mọi chữ cái trong <code>b</code> đều xuất hiện trong <code>a</code>, tính cả số lần xuất hiện.</p>

<ul>
	<li>Ví dụ, <code>&quot;wrr&quot;</code> là tập con của <code>&quot;warrior&quot;</code> nhưng không phải tập con của <code>&quot;world&quot;</code>.</li>
</ul>

<p>Chuỗi <code>a</code> trong <code>words1</code> được gọi là <strong>phổ quát</strong> nếu với mọi chuỗi <code>b</code> trong <code>words2</code>, <code>b</code> đều là tập con của <code>a</code>.</p>

<p>Hãy trả về mảng chứa tất cả chuỗi <strong>phổ quát</strong> trong <code>words1</code>. Có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words1 = [&quot;amazon&quot;,&quot;apple&quot;,&quot;facebook&quot;,&quot;google&quot;,&quot;leetcode&quot;], words2 = [&quot;e&quot;,&quot;o&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;facebook&quot;,&quot;google&quot;,&quot;leetcode&quot;]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words1 = [&quot;amazon&quot;,&quot;apple&quot;,&quot;facebook&quot;,&quot;google&quot;,&quot;leetcode&quot;], words2 = [&quot;lc&quot;,&quot;eo&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;leetcode&quot;]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words1 = [&quot;acaac&quot;,&quot;cccbb&quot;,&quot;aacbb&quot;,&quot;caacc&quot;,&quot;bcbbb&quot;], words2 = [&quot;c&quot;,&quot;cc&quot;,&quot;b&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;cccbb&quot;]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words1.length, words2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words1[i].length, words2[i].length &lt;= 10</code></li>
	<li><code>words1[i]</code> và <code>words2[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả chuỗi trong <code>words1</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $a$ phải đáp ứng mọi chuỗi trong $words2$. Có thể kiểm tra từng $b$ với từng $a$, nhưng ta gộp các ràng buộc: với mỗi chữ cái, lấy số lần xuất hiện lớn nhất trong toàn bộ $words2$, rồi so sánh bộ đếm duy nhất $\textit{cnt}$ đó với số lần xuất hiện trong $a$.

<!-- thinking:end -->

Duyệt từng chuỗi `b` trong `words2`, đếm số lần xuất hiện lớn nhất của mỗi chữ cái và lưu vào `cnt`.

Sau đó, duyệt từng chuỗi `a` trong `words1`, đếm số lần xuất hiện của mỗi chữ cái và lưu vào `t`. Nếu số lần xuất hiện của mỗi chữ cái trong `cnt` không lớn hơn số lần tương ứng trong `t`, thì `a` là chuỗi phổ quát; thêm nó vào đáp án.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả chuỗi trong `words1` và `words2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wordSubsets(self, words1: List[str], words2: List[str]) -> List[str]:
        cnt = Counter()
        for b in words2:
            t = Counter(b)
            for c, v in t.items():
                cnt[c] = max(cnt[c], v)
        ans = []
        for a in words1:
            t = Counter(a)
            if all(v <= t[c] for c, v in cnt.items()):
                ans.append(a)
        return ans
```

#### Java

```java
class Solution {
    public List<String> wordSubsets(String[] words1, String[] words2) {
        int[] cnt = new int[26];
        for (var b : words2) {
            int[] t = new int[26];
            for (int i = 0; i < b.length(); ++i) {
                t[b.charAt(i) - 'a']++;
            }
            for (int i = 0; i < 26; ++i) {
                cnt[i] = Math.max(cnt[i], t[i]);
            }
        }
        List<String> ans = new ArrayList<>();
        for (var a : words1) {
            int[] t = new int[26];
            for (int i = 0; i < a.length(); ++i) {
                t[a.charAt(i) - 'a']++;
            }
            boolean ok = true;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > t[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.add(a);
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
    vector<string> wordSubsets(vector<string>& words1, vector<string>& words2) {
        int cnt[26] = {0};
        int t[26];
        for (auto& b : words2) {
            memset(t, 0, sizeof t);
            for (auto& c : b) {
                t[c - 'a']++;
            }
            for (int i = 0; i < 26; ++i) {
                cnt[i] = max(cnt[i], t[i]);
            }
        }
        vector<string> ans;
        for (auto& a : words1) {
            memset(t, 0, sizeof t);
            for (auto& c : a) {
                t[c - 'a']++;
            }
            bool ok = true;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > t[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.emplace_back(a);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func wordSubsets(words1 []string, words2 []string) (ans []string) {
	cnt := [26]int{}
	for _, b := range words2 {
		t := [26]int{}
		for _, c := range b {
			t[c-'a']++
		}
		for i := range cnt {
			cnt[i] = max(cnt[i], t[i])
		}
	}
	for _, a := range words1 {
		t := [26]int{}
		for _, c := range a {
			t[c-'a']++
		}
		ok := true
		for i, v := range cnt {
			if v > t[i] {
				ok = false
				break
			}
		}
		if ok {
			ans = append(ans, a)
		}
	}
	return
}
```

#### TypeScript

```ts
function wordSubsets(words1: string[], words2: string[]): string[] {
    const cnt: number[] = Array(26).fill(0);
    for (const b of words2) {
        const t: number[] = Array(26).fill(0);
        for (const c of b) {
            t[c.charCodeAt(0) - 97]++;
        }
        for (let i = 0; i < 26; i++) {
            cnt[i] = Math.max(cnt[i], t[i]);
        }
    }

    const ans: string[] = [];
    for (const a of words1) {
        const t: number[] = Array(26).fill(0);
        for (const c of a) {
            t[c.charCodeAt(0) - 97]++;
        }

        let ok = true;
        for (let i = 0; i < 26; i++) {
            if (cnt[i] > t[i]) {
                ok = false;
                break;
            }
        }

        if (ok) {
            ans.push(a);
        }
    }

    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string[]} words1
 * @param {string[]} words2
 * @return {string[]}
 */
var wordSubsets = function (words1, words2) {
    const cnt = Array(26).fill(0);

    for (const b of words2) {
        const t = Array(26).fill(0);

        for (const c of b) {
            t[c.charCodeAt(0) - 97]++;
        }

        for (let i = 0; i < 26; i++) {
            cnt[i] = Math.max(cnt[i], t[i]);
        }
    }

    const ans = [];

    for (const a of words1) {
        const t = Array(26).fill(0);

        for (const c of a) {
            t[c.charCodeAt(0) - 97]++;
        }

        let ok = true;
        for (let i = 0; i < 26; i++) {
            if (cnt[i] > t[i]) {
                ok = false;
                break;
            }
        }

        if (ok) {
            ans.push(a);
        }
    }

    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
