---
comments: true
difficulty: Medium
rating: 1548
source: Weekly Contest 317 Q2
tags:
    - Array
    - Hash Table
    - String
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2456. Most Popular Video Creator](https://leetcode.com/problems/most-popular-video-creator)

[中文文档](/solution/2400-2499/2456.Most%20Popular%20Video%20Creator/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp hai mảng chuỗi <code>creators</code> và <code>ids</code>, cùng một mảng số nguyên <code>views</code>, tất cả đều có độ dài <code>n</code>. Video thứ <code>i<sup>th</sup></code> trên một nền tảng được tạo bởi <code>creators[i]</code>, có id là <code>ids[i]</code> và có <code>views[i]</code> lượt xem.</p>

<p><strong>Độ phổ biến</strong> của một creator là <strong>tổng số lượt xem</strong> của <strong>tất cả</strong> video do creator đó tạo ra. Hãy tìm creator có độ phổ biến <strong>cao nhất</strong> và id của video có <strong>nhiều</strong> lượt xem nhất của creator đó.</p>

<ul>
	<li>Nếu có nhiều creator cùng có độ phổ biến cao nhất, hãy tìm tất cả các creator đó.</li>
	<li>Nếu một creator có nhiều video cùng có số lượt xem cao nhất, hãy tìm id <strong>nhỏ nhất theo thứ tự từ điển</strong>.</li>
</ul>

<p>Lưu ý: Các video khác nhau có thể có cùng một <code>id</code>, nghĩa là <code>id</code> không nhất thiết định danh duy nhất một video. Ví dụ, hai video có cùng ID vẫn được xem là hai video riêng biệt với số lượt xem riêng.</p>

<p>Trả về<em> </em>một <strong>mảng 2 chiều</strong> các <strong>chuỗi</strong> <code>answer</code>, trong đó <code>answer[i] = [creators<sub>i</sub>, id<sub>i</sub>]</code> nghĩa là <code>creators<sub>i</sub></code> có độ phổ biến <strong>cao nhất</strong> và <code>id<sub>i</sub></code> là <strong>id</strong> của video <strong>phổ biến nhất</strong> của creator đó. Có thể trả về đáp án theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">creators = [&quot;alice&quot;,&quot;bob&quot;,&quot;alice&quot;,&quot;chris&quot;], ids = [&quot;one&quot;,&quot;two&quot;,&quot;three&quot;,&quot;four&quot;], views = [5,10,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[&quot;alice&quot;,&quot;one&quot;],[&quot;bob&quot;,&quot;two&quot;]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Độ phổ biến của alice là 5 + 5 = 10.<br />
Độ phổ biến của bob là 10.<br />
Độ phổ biến của chris là 4.<br />
alice và bob là hai creator phổ biến nhất.<br />
Với bob, video có số lượt xem cao nhất là &quot;two&quot;.<br />
Với alice, các video có số lượt xem cao nhất là &quot;one&quot; và &quot;three&quot;. Vì &quot;one&quot; nhỏ hơn &quot;three&quot; theo thứ tự từ điển, nên &quot;one&quot; được đưa vào đáp án.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">creators = [&quot;alice&quot;,&quot;alice&quot;,&quot;alice&quot;], ids = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;], views = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[&quot;alice&quot;,&quot;b&quot;]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các video có id &quot;b&quot; và &quot;c&quot; có số lượt xem cao nhất.<br />
Vì &quot;b&quot; nhỏ hơn &quot;c&quot; theo thứ tự từ điển, nên &quot;b&quot; được đưa vào đáp án.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == creators.length == ids.length == views.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= creators[i].length, ids[i].length &lt;= 5</code></li>
	<li><code>creators[i]</code> và <code>ids[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= views[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^5$, ta cộng dồn lượt xem theo từng creator và lưu video có nhiều lượt xem nhất của creator đó (nếu hòa thì chọn id nhỏ nhất theo thứ tự từ điển). $cnt$ lưu tổng lượt xem; $d$ lưu chỉ số của video tốt nhất. Sau đó, lấy tổng lớn nhất và đưa vào đáp án mọi creator có tổng bằng giá trị đó.

<!-- thinking:end -->

Ta duyệt qua ba mảng, dùng hash table $cnt$ để tính tổng lượt xem của mỗi creator, đồng thời dùng hash table $d$ để lưu chỉ số của video có số lượt xem cao nhất của mỗi creator.

Sau đó, ta duyệt hash table $cnt$ để tìm số lượt xem lớn nhất $mx$; rồi duyệt hash table $cnt$ một lần nữa để tìm các creator có số lượt xem bằng $mx$ và thêm họ vào mảng đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng video.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostPopularCreator(
        self, creators: List[str], ids: List[str], views: List[int]
    ) -> List[List[str]]:
        cnt = defaultdict(int)
        d = defaultdict(int)
        for k, (c, i, v) in enumerate(zip(creators, ids, views)):
            cnt[c] += v
            if c not in d or views[d[c]] < v or (views[d[c]] == v and ids[d[c]] > i):
                d[c] = k
        mx = max(cnt.values())
        return [[c, ids[d[c]]] for c, x in cnt.items() if x == mx]
```

#### Java

```java
class Solution {
    public List<List<String>> mostPopularCreator(String[] creators, String[] ids, int[] views) {
        int n = ids.length;
        Map<String, Long> cnt = new HashMap<>(n);
        Map<String, Integer> d = new HashMap<>(n);
        for (int k = 0; k < n; ++k) {
            String c = creators[k], i = ids[k];
            long v = views[k];
            cnt.merge(c, v, Long::sum);
            if (!d.containsKey(c) || views[d.get(c)] < v
                || (views[d.get(c)] == v && ids[d.get(c)].compareTo(i) > 0)) {
                d.put(c, k);
            }
        }
        long mx = 0;
        for (long x : cnt.values()) {
            mx = Math.max(mx, x);
        }
        List<List<String>> ans = new ArrayList<>();
        for (var e : cnt.entrySet()) {
            if (e.getValue() == mx) {
                String c = e.getKey();
                ans.add(List.of(c, ids[d.get(c)]));
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
    vector<vector<string>> mostPopularCreator(vector<string>& creators, vector<string>& ids, vector<int>& views) {
        unordered_map<string, long long> cnt;
        unordered_map<string, int> d;
        int n = ids.size();
        for (int k = 0; k < n; ++k) {
            auto c = creators[k], id = ids[k];
            int v = views[k];
            cnt[c] += v;
            if (!d.count(c) || views[d[c]] < v || (views[d[c]] == v && ids[d[c]] > id)) {
                d[c] = k;
            }
        }
        long long mx = 0;
        for (auto& [_, x] : cnt) {
            mx = max(mx, x);
        }
        vector<vector<string>> ans;
        for (auto& [c, x] : cnt) {
            if (x == mx) {
                ans.push_back({c, ids[d[c]]});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mostPopularCreator(creators []string, ids []string, views []int) (ans [][]string) {
	cnt := map[string]int{}
	d := map[string]int{}
	for k, c := range creators {
		i, v := ids[k], views[k]
		cnt[c] += v
		if j, ok := d[c]; !ok || views[j] < v || (views[j] == v && ids[j] > i) {
			d[c] = k
		}
	}
	mx := 0
	for _, x := range cnt {
		if mx < x {
			mx = x
		}
	}
	for c, x := range cnt {
		if x == mx {
			ans = append(ans, []string{c, ids[d[c]]})
		}
	}
	return
}
```

#### TypeScript

```ts
function mostPopularCreator(creators: string[], ids: string[], views: number[]): string[][] {
    const cnt: Map<string, number> = new Map();
    const d: Map<string, number> = new Map();
    const n = ids.length;
    for (let k = 0; k < n; ++k) {
        const [c, i, v] = [creators[k], ids[k], views[k]];
        cnt.set(c, (cnt.get(c) ?? 0) + v);
        if (!d.has(c) || views[d.get(c)!] < v || (views[d.get(c)!] === v && ids[d.get(c)!] > i)) {
            d.set(c, k);
        }
    }
    const mx = Math.max(...cnt.values());
    const ans: string[][] = [];
    for (const [c, x] of cnt) {
        if (x === mx) {
            ans.push([c, ids[d.get(c)!]]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
