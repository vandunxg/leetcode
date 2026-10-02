---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [811. Subdomain Visit Count](https://leetcode.com/problems/subdomain-visit-count)

[中文文档](/solution/0800-0899/0811.Subdomain%20Visit%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Một domain website như <code>&quot;discuss.leetcode.com&quot;</code> gồm nhiều subdomain. Ở cấp cao nhất là <code>&quot;com&quot;</code>, cấp tiếp theo là <code>&quot;leetcode.com&quot;</code>&nbsp;và cấp thấp nhất là <code>&quot;discuss.leetcode.com&quot;</code>. Khi truy cập domain như <code>&quot;discuss.leetcode.com&quot;</code>, ta cũng ngầm truy cập các domain cha là <code>&quot;leetcode.com&quot;</code> và <code>&quot;com&quot;</code>.</p>

<p><strong>Domain kèm số lượt truy cập</strong> là domain có một trong hai định dạng <code>&quot;rep d1.d2.d3&quot;</code> hoặc <code>&quot;rep d1.d2&quot;</code>, trong đó <code>rep</code> là số lượt truy cập domain và <code>d1.d2.d3</code> là tên domain.</p>

<ul>
	<li>Ví dụ, <code>&quot;9001 discuss.leetcode.com&quot;</code> là một <strong>domain kèm số lượt truy cập</strong>, cho biết <code>discuss.leetcode.com</code> đã được truy cập <code>9001</code> lần.</li>
</ul>

<p>Cho mảng <code>cpdomains</code> gồm các <strong>domain kèm số lượt truy cập</strong>, hãy trả về <em>mảng chứa <strong>số lượt truy cập kèm domain</strong> cho từng subdomain trong đầu vào</em>. Có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cpdomains = [&quot;9001 discuss.leetcode.com&quot;]
<strong>Đầu ra:</strong> [&quot;9001 leetcode.com&quot;,&quot;9001 discuss.leetcode.com&quot;,&quot;9001 com&quot;]
<strong>Giải thích:</strong> Ta chỉ có một domain website: &quot;discuss.leetcode.com&quot;.
Như đã nói ở trên, các subdomain &quot;leetcode.com&quot; và &quot;com&quot; cũng được truy cập. Vì vậy, mỗi domain đều được truy cập 9001 lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cpdomains = [&quot;900 google.mail.com&quot;, &quot;50 yahoo.com&quot;, &quot;1 intel.mail.com&quot;, &quot;5 wiki.org&quot;]
<strong>Đầu ra:</strong> [&quot;901 mail.com&quot;,&quot;50 yahoo.com&quot;,&quot;900 google.mail.com&quot;,&quot;5 wiki.org&quot;,&quot;5 org&quot;,&quot;1 intel.mail.com&quot;,&quot;951 com&quot;]
<strong>Giải thích:</strong> Ta sẽ truy cập &quot;google.mail.com&quot; 900 lần, &quot;yahoo.com&quot; 50 lần, &quot;intel.mail.com&quot; 1 lần và &quot;wiki.org&quot; 5 lần.
Với các subdomain, ta sẽ truy cập &quot;mail.com&quot; 900 + 1 = 901 lần, &quot;com&quot; 900 + 50 + 1 = 951 lần và &quot;org&quot; 5 lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cpdomain.length &lt;= 100</code></li>
	<li><code>1 &lt;= cpdomain[i].length &lt;= 100</code></li>
	<li><code>cpdomain[i]</code> có định dạng <code>&quot;rep<sub>i</sub> d1<sub>i</sub>.d2<sub>i</sub>.d3<sub>i</sub>&quot;</code> hoặc <code>&quot;rep<sub>i</sub> d1<sub>i</sub>.d2<sub>i</sub>&quot;</code>.</li>
	<li><code>rep<sub>i</sub></code> là số nguyên trong khoảng <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>d1<sub>i</sub></code>, <code>d2<sub>i</sub></code> và <code>d3<sub>i</sub></code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần cộng số lượt truy cập cho domain đó và mọi domain cha của nó. Có tối đa $100$ bản ghi ngắn, nên chỉ cần tách chuỗi theo dấu chấm.
>
> Với mỗi hậu tố bắt đầu sau dấu cách hoặc dấu chấm, cộng số lượt truy cập tương ứng. Sau đó định dạng bộ đếm thành các chuỗi theo yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subdomainVisits(self, cpdomains: List[str]) -> List[str]:
        cnt = Counter()
        for s in cpdomains:
            v = int(s[: s.index(' ')])
            for i, c in enumerate(s):
                if c in ' .':
                    cnt[s[i + 1 :]] += v
        return [f'{v} {s}' for s, v in cnt.items()]
```

#### Java

```java
class Solution {
    public List<String> subdomainVisits(String[] cpdomains) {
        Map<String, Integer> cnt = new HashMap<>();
        for (String s : cpdomains) {
            int i = s.indexOf(" ");
            int v = Integer.parseInt(s.substring(0, i));
            for (; i < s.length(); ++i) {
                if (s.charAt(i) == ' ' || s.charAt(i) == '.') {
                    String t = s.substring(i + 1);
                    cnt.put(t, cnt.getOrDefault(t, 0) + v);
                }
            }
        }
        List<String> ans = new ArrayList<>();
        for (var e : cnt.entrySet()) {
            ans.add(e.getValue() + " " + e.getKey());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> subdomainVisits(vector<string>& cpdomains) {
        unordered_map<string, int> cnt;
        for (auto& s : cpdomains) {
            int i = s.find(' ');
            int v = stoi(s.substr(0, i));
            for (; i < s.size(); ++i) {
                if (s[i] == ' ' || s[i] == '.') {
                    cnt[s.substr(i + 1)] += v;
                }
            }
        }
        vector<string> ans;
        for (auto& [s, v] : cnt) {
            ans.push_back(to_string(v) + " " + s);
        }
        return ans;
    }
};
```

#### Go

```go
func subdomainVisits(cpdomains []string) []string {
	cnt := map[string]int{}
	for _, s := range cpdomains {
		i := strings.IndexByte(s, ' ')
		v, _ := strconv.Atoi(s[:i])
		for ; i < len(s); i++ {
			if s[i] == ' ' || s[i] == '.' {
				cnt[s[i+1:]] += v
			}
		}
	}
	ans := make([]string, 0, len(cnt))
	for s, v := range cnt {
		ans = append(ans, strconv.Itoa(v)+" "+s)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
