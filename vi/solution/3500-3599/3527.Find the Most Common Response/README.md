---
comments: true
difficulty: Medium
rating: 1282
source: Biweekly Contest 155 Q1
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3527. Find the Most Common Response](https://leetcode.com/problems/find-the-most-common-response)

[中文文档](/solution/3500-3599/3527.Find%20the%20Most%20Common%20Response/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi 2D <code>responses</code>, trong đó mỗi <code>responses[i]</code> là một mảng chuỗi biểu diễn các câu trả lời khảo sát từ ngày thứ <code>i<sup>th</sup></code>.</p>

<p>Hãy trả về câu trả lời <strong>phổ biến nhất</strong> trong tất cả các ngày sau khi loại bỏ các câu trả lời <strong>trùng lặp</strong> trong mỗi <code>responses[i]</code>. Nếu có nhiều câu trả lời cùng số lần xuất hiện, hãy trả về câu trả lời <em><span data-keyword="lexicographically-smaller-string">nhỏ hơn theo thứ tự từ điển</span></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">responses = [[&quot;good&quot;,&quot;ok&quot;,&quot;good&quot;,&quot;ok&quot;],[&quot;ok&quot;,&quot;bad&quot;,&quot;good&quot;,&quot;ok&quot;,&quot;ok&quot;],[&quot;good&quot;],[&quot;bad&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;good&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Sau khi loại bỏ các phần tử trùng lặp trong mỗi danh sách, <code>responses = [[&quot;good&quot;, &quot;ok&quot;], [&quot;ok&quot;, &quot;bad&quot;, &quot;good&quot;], [&quot;good&quot;], [&quot;bad&quot;]]</code>.</li>
    <li><code>&quot;good&quot;</code> xuất hiện 3 lần, <code>&quot;ok&quot;</code> xuất hiện 2 lần và <code>&quot;bad&quot;</code> xuất hiện 2 lần.</li>
    <li>Trả về <code>&quot;good&quot;</code> vì nó có tần suất cao nhất.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">responses = [[&quot;good&quot;,&quot;ok&quot;,&quot;good&quot;],[&quot;ok&quot;,&quot;bad&quot;],[&quot;bad&quot;,&quot;notsure&quot;],[&quot;great&quot;,&quot;good&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;bad&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Sau khi loại bỏ các phần tử trùng lặp trong mỗi danh sách, ta có <code>responses = [[&quot;good&quot;, &quot;ok&quot;], [&quot;ok&quot;, &quot;bad&quot;], [&quot;bad&quot;, &quot;notsure&quot;], [&quot;great&quot;, &quot;good&quot;]]</code>.</li>
    <li><code>&quot;bad&quot;</code>, <code>&quot;good&quot;</code> và <code>&quot;ok&quot;</code> đều xuất hiện 2 lần.</li>
    <li>Kết quả là <code>&quot;bad&quot;</code> vì đây là từ nhỏ nhất theo thứ tự từ điển trong các từ có tần suất cao nhất.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= responses.length &lt;= 1000</code></li>
    <li><code>1 &lt;= responses[i].length &lt;= 1000</code></li>
    <li><code>1 &lt;= responses[i][j].length &lt;= 10</code></li>
    <li><code>responses[i][j]</code> chỉ gồm các chữ cái tiếng Anh viết thường</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các câu trả lời trùng lặp trong cùng một ngày chỉ được tính một lần. Trên tất cả các ngày, ta cần tìm chuỗi xuất hiện nhiều nhất, nếu hòa thì chọn chuỗi nhỏ hơn theo thứ tự từ điển.
>
> Dùng một set để loại trùng trong từng ngày, cộng dồn vào một hash map, rồi duyệt để tìm số lần xuất hiện và chuỗi tốt nhất. Công việc có độ phức tạp tuyến tính theo tổng độ dài.

<!-- thinking:end -->

Ta có thể dùng một bảng băm $\textit{cnt}$ để đếm số lần xuất hiện của mỗi câu trả lời. Với các câu trả lời của từng ngày, trước tiên ta loại bỏ các phần tử trùng lặp, sau đó thêm từng câu trả lời vào bảng băm và cập nhật số lần xuất hiện.

Cuối cùng, ta duyệt qua bảng băm để tìm câu trả lời có số lần xuất hiện lớn nhất. Nếu có nhiều câu trả lời cùng số lần xuất hiện, ta trả về câu trả lời nhỏ nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(L)$, và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các câu trả lời.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCommonResponse(self, responses: List[List[str]]) -> str:
        cnt = Counter()
        for ws in responses:
            for w in set(ws):
                cnt[w] += 1
        ans = responses[0][0]
        for w, x in cnt.items():
            if cnt[ans] < x or (cnt[ans] == x and w < ans):
                ans = w
        return ans
```

#### Java

```java
class Solution {
    public String findCommonResponse(List<List<String>> responses) {
        Map<String, Integer> cnt = new HashMap<>();
        for (var ws : responses) {
            Set<String> s = new HashSet<>();
            for (var w : ws) {
                if (s.add(w)) {
                    cnt.merge(w, 1, Integer::sum);
                }
            }
        }
        String ans = responses.get(0).get(0);
        for (var e : cnt.entrySet()) {
            String w = e.getKey();
            int v = e.getValue();
            if (cnt.get(ans) < v || (cnt.get(ans) == v && w.compareTo(ans) < 0)) {
                ans = w;
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
    string findCommonResponse(vector<vector<string>>& responses) {
        unordered_map<string, int> cnt;
        for (const auto& ws : responses) {
            unordered_set<string> s;
            for (const auto& w : ws) {
                if (s.insert(w).second) {
                    ++cnt[w];
                }
            }
        }
        string ans = responses[0][0];
        for (const auto& e : cnt) {
            const string& w = e.first;
            int v = e.second;
            if (cnt[ans] < v || (cnt[ans] == v && w < ans)) {
                ans = w;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findCommonResponse(responses [][]string) string {
    cnt := map[string]int{}
    for _, ws := range responses {
        s := map[string]struct{}{}
        for _, w := range ws {
            if _, ok := s[w]; !ok {
                s[w] = struct{}{}
                cnt[w]++
            }
        }
    }
    ans := responses[0][0]
    for w, v := range cnt {
        if cnt[ans] < v || (cnt[ans] == v && w < ans) {
            ans = w
        }
    }
    return ans
}
```

#### TypeScript

```ts
function findCommonResponse(responses: string[][]): string {
    const cnt = new Map<string, number>();
    for (const ws of responses) {
        const s = new Set<string>();
        for (const w of ws) {
            if (!s.has(w)) {
                s.add(w);
                cnt.set(w, (cnt.get(w) ?? 0) + 1);
            }
        }
    }
    let ans = responses[0][0];
    for (const [w, v] of cnt) {
        const best = cnt.get(ans)!;
        if (best < v || (best === v && w < ans)) {
            ans = w;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
