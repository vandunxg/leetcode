---
comments: true
difficulty: Easy
rating: 1307
source: Biweekly Contest 66 Q1
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2085. Count Common Words With One Occurrence](https://leetcode.com/problems/count-common-words-with-one-occurrence)

[中文文档](/solution/2000-2099/2085.Count%20Common%20Words%20With%20One%20Occurrence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>words1</code> và <code>words2</code>, hãy trả về <em>số lượng chuỗi xuất hiện <strong>chính xác một lần</strong> trong <b>cả</b>&nbsp;hai mảng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words1 = [&quot;leetcode&quot;,&quot;is&quot;,&quot;amazing&quot;,&quot;as&quot;,&quot;is&quot;], words2 = [&quot;amazing&quot;,&quot;leetcode&quot;,&quot;is&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- &quot;leetcode&quot; xuất hiện chính xác một lần trong mỗi mảng. Ta tính chuỗi này.
- &quot;amazing&quot; xuất hiện chính xác một lần trong mỗi mảng. Ta tính chuỗi này.
- &quot;is&quot; xuất hiện trong mỗi mảng, nhưng xuất hiện 2 lần trong words1. Ta không tính chuỗi này.
- &quot;as&quot; xuất hiện một lần trong words1 nhưng không xuất hiện trong words2. Ta không tính chuỗi này.
Vậy có 2 chuỗi xuất hiện chính xác một lần trong mỗi mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words1 = [&quot;b&quot;,&quot;bb&quot;,&quot;bbb&quot;], words2 = [&quot;a&quot;,&quot;aa&quot;,&quot;aaa&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chuỗi nào xuất hiện trong cả hai mảng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words1 = [&quot;a&quot;,&quot;ab&quot;], words2 = [&quot;a&quot;,&quot;a&quot;,&quot;a&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chuỗi duy nhất xuất hiện chính xác một lần trong mỗi mảng là &quot;ab&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words1.length, words2.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words1[i].length, words2[j].length &lt;= 30</code></li>
	<li><code>words1[i]</code> và <code>words2[j]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Counting

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các từ xuất hiện một lần trong mỗi mảng. Xây dựng hai bộ đếm, sau đó duyệt một trong hai bộ và yêu cầu cả hai tần suất đều bằng $1$.
>
> Độ phức tạp tuyến tính theo độ dài của hai mảng.

<!-- thinking:end -->

Ta có thể sử dụng hai hash table $cnt1$ và $cnt2$ để đếm số lần xuất hiện của mỗi chuỗi trong hai mảng chuỗi tương ứng. Sau đó, ta duyệt qua một trong hai hash table. Nếu một chuỗi xuất hiện một lần trong hash table còn lại và cũng xuất hiện một lần trong hash table hiện tại, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của hai mảng chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countWords(self, words1: List[str], words2: List[str]) -> int:
        cnt1 = Counter(words1)
        cnt2 = Counter(words2)
        return sum(v == 1 and cnt2[w] == 1 for w, v in cnt1.items())
```

#### Java

```java
class Solution {
    public int countWords(String[] words1, String[] words2) {
        Map<String, Integer> cnt1 = new HashMap<>();
        Map<String, Integer> cnt2 = new HashMap<>();
        for (var w : words1) {
            cnt1.merge(w, 1, Integer::sum);
        }
        for (var w : words2) {
            cnt2.merge(w, 1, Integer::sum);
        }
        int ans = 0;
        for (var e : cnt1.entrySet()) {
            if (e.getValue() == 1 && cnt2.getOrDefault(e.getKey(), 0) == 1) {
                ++ans;
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
    int countWords(vector<string>& words1, vector<string>& words2) {
        unordered_map<string, int> cnt1;
        unordered_map<string, int> cnt2;
        for (auto& w : words1) {
            ++cnt1[w];
        }
        for (auto& w : words2) {
            ++cnt2[w];
        }
        int ans = 0;
        for (auto& [w, v] : cnt1) {
            ans += v == 1 && cnt2[w] == 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countWords(words1 []string, words2 []string) (ans int) {
	cnt1 := map[string]int{}
	cnt2 := map[string]int{}
	for _, w := range words1 {
		cnt1[w]++
	}
	for _, w := range words2 {
		cnt2[w]++
	}
	for w, v := range cnt1 {
		if v == 1 && cnt2[w] == 1 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countWords(words1: string[], words2: string[]): number {
    const cnt1 = new Map<string, number>();
    const cnt2 = new Map<string, number>();
    for (const w of words1) {
        cnt1.set(w, (cnt1.get(w) ?? 0) + 1);
    }
    for (const w of words2) {
        cnt2.set(w, (cnt2.get(w) ?? 0) + 1);
    }
    let ans = 0;
    for (const [w, v] of cnt1) {
        if (v === 1 && cnt2.get(w) === 1) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
