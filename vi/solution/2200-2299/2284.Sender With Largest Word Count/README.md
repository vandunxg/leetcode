---
comments: true
difficulty: Medium
rating: 1346
source: Biweekly Contest 79 Q2
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2284. Sender With Largest Word Count](https://leetcode.com/problems/sender-with-largest-word-count)

[中文文档](/solution/2200-2299/2284.Sender%20With%20Largest%20Word%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có nhật ký trò chuyện gồm <code>n</code> tin nhắn. Cho hai mảng chuỗi <code>messages</code> và <code>senders</code>, trong đó <code>messages[i]</code> là một <strong>tin nhắn</strong> được gửi bởi <code>senders[i]</code>.</p>

<p>Một <strong>tin nhắn</strong> là danh sách các <strong>từ</strong> được phân tách bằng một dấu cách duy nhất, không có dấu cách ở đầu hoặc cuối. <strong>Số từ</strong> của một người gửi là tổng số <strong>từ</strong> mà người đó đã gửi. Lưu ý rằng một người gửi có thể gửi nhiều tin nhắn.</p>

<p>Hãy trả về <em>người gửi có <strong>số từ</strong> lớn nhất</em>. Nếu có nhiều người gửi cùng có số từ lớn nhất, hãy trả về <em>người có tên <strong>lớn nhất theo thứ tự từ điển</strong></em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Trong thứ tự từ điển, chữ cái viết hoa đứng trước chữ cái viết thường.</li>
	<li><code>&quot;Alice&quot;</code> và <code>&quot;alice&quot;</code> là hai tên khác nhau.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> messages = [&quot;Hello userTwooo&quot;,&quot;Hi userThree&quot;,&quot;Wonderful day Alice&quot;,&quot;Nice day userThree&quot;], senders = [&quot;Alice&quot;,&quot;userTwo&quot;,&quot;userThree&quot;,&quot;Alice&quot;]
<strong>Đầu ra:</strong> &quot;Alice&quot;
<strong>Giải thích:</strong> Alice gửi tổng cộng 2 + 3 = 5 từ.
userTwo gửi tổng cộng 2 từ.
userThree gửi tổng cộng 3 từ.
Vì Alice có số từ lớn nhất, ta trả về &quot;Alice&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> messages = [&quot;How is leetcode for everyone&quot;,&quot;Leetcode is useful for practice&quot;], senders = [&quot;Bob&quot;,&quot;Charlie&quot;]
<strong>Đầu ra:</strong> &quot;Charlie&quot;
<strong>Giải thích:</strong> Bob gửi tổng cộng 5 từ.
Charlie gửi tổng cộng 5 từ.
Vì có nhiều người cùng có số từ lớn nhất, ta trả về người gửi có tên lớn hơn theo thứ tự từ điển, đó là Charlie.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == messages.length == senders.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= messages[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= senders[i].length &lt;= 10</code></li>
	<li><code>messages[i]</code> chỉ gồm các chữ cái tiếng Anh viết hoa, viết thường và ký tự <code>&#39; &#39;</code>.</li>
	<li>Tất cả các từ trong <code>messages[i]</code> được phân tách bằng <strong>một dấu cách duy nhất</strong>.</li>
	<li><code>messages[i]</code> không có dấu cách ở đầu hoặc cuối.</li>
	<li><code>senders[i]</code> chỉ gồm các chữ cái tiếng Anh viết hoa và viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Ta cộng tổng số từ của từng người gửi và cần tìm giá trị lớn nhất; nếu hòa, ưu tiên tên lớn hơn theo thứ tự từ điển. Có $10^4$ tin nhắn, nên chỉ cần tách chuỗi theo dấu cách.
>
> Số từ bằng số dấu cách cộng một. Ta dùng hash map để cộng dồn tổng số từ, sau đó duyệt tuyến tính để giữ lại tên tốt nhất hiện tại.

<!-- thinking:end -->

Ta có thể sử dụng bảng băm $\textit{cnt}$ để lưu số từ của mỗi người gửi. Sau đó, ta duyệt qua bảng băm để tìm người gửi có số từ lớn nhất. Nếu có nhiều người gửi cùng có số từ lớn nhất, ta trả về tên lớn nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n + L)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số tin nhắn và $L$ là tổng độ dài của tất cả tin nhắn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestWordCount(self, messages: List[str], senders: List[str]) -> str:
        cnt = Counter()
        for message, sender in zip(messages, senders):
            cnt[sender] += message.count(" ") + 1
        ans = senders[0]
        for k, v in cnt.items():
            if cnt[ans] < v or (cnt[ans] == v and ans < k):
                ans = k
        return ans
```

#### Java

```java
class Solution {
    public String largestWordCount(String[] messages, String[] senders) {
        Map<String, Integer> cnt = new HashMap<>(senders.length);
        for (int i = 0; i < messages.length; ++i) {
            int v = 1;
            for (int j = 0; j < messages[i].length(); ++j) {
                if (messages[i].charAt(j) == ' ') {
                    ++v;
                }
            }
            cnt.merge(senders[i], v, Integer::sum);
        }
        String ans = senders[0];
        for (var e : cnt.entrySet()) {
            String k = e.getKey();
            int v = e.getValue();
            if (cnt.get(ans) < v || (cnt.get(ans) == v && ans.compareTo(k) < 0)) {
                ans = k;
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
    string largestWordCount(vector<string>& messages, vector<string>& senders) {
        unordered_map<string, int> cnt;
        for (int i = 0; i < messages.size(); ++i) {
            int v = count(messages[i].begin(), messages[i].end(), ' ') + 1;
            cnt[senders[i]] += v;
        }
        string ans = senders[0];
        for (auto& [k, v] : cnt) {
            if (cnt[ans] < v || (cnt[ans] == v && ans < k)) {
                ans = k;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestWordCount(messages []string, senders []string) string {
	cnt := make(map[string]int)
	for i, message := range messages {
		v := strings.Count(message, " ") + 1
		cnt[senders[i]] += v
	}

	ans := senders[0]
	for k, v := range cnt {
		if cnt[ans] < v || (cnt[ans] == v && ans < k) {
			ans = k
		}
	}
	return ans
}
```

#### TypeScript

```ts
function largestWordCount(messages: string[], senders: string[]): string {
    const cnt: { [key: string]: number } = {};

    for (let i = 0; i < messages.length; ++i) {
        const v = messages[i].split(' ').length;
        cnt[senders[i]] = (cnt[senders[i]] || 0) + v;
    }

    let ans = senders[0];
    for (const k in cnt) {
        if (cnt[ans] < cnt[k] || (cnt[ans] === cnt[k] && ans < k)) {
            ans = k;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
