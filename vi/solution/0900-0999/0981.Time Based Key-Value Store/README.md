---
comments: true
difficulty: Medium
tags:
    - Design
    - Hash Table
    - String
    - Binary Search
---

<!-- problem:start -->

# [981. Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store)

[中文文档](/solution/0900-0999/0981.Time%20Based%20Key-Value%20Store/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một cấu trúc dữ liệu key-value theo thời gian, có thể lưu nhiều giá trị cho cùng một key ở các timestamp khác nhau và truy xuất giá trị của key tại một timestamp nhất định.</p>

<p>Triển khai class <code>TimeMap</code>:</p>

<ul>
	<li><code>TimeMap()</code> Khởi tạo đối tượng của cấu trúc dữ liệu.</li>
	<li><code>void set(String key, String value, int timestamp)</code> Lưu key <code>key</code> cùng giá trị <code>value</code> tại thời điểm <code>timestamp</code> đã cho.</li>
	<li><code>String get(String key, int timestamp)</code> Trả về một giá trị đã được lưu bằng lời gọi <code>set</code> trước đó, với <code>timestamp_prev &lt;= timestamp</code>. Nếu có nhiều giá trị như vậy, trả về giá trị ứng với <code>timestamp_prev</code> lớn nhất. Nếu không có giá trị nào, trả về <code>&quot;&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;TimeMap&quot;, &quot;set&quot;, &quot;get&quot;, &quot;get&quot;, &quot;set&quot;, &quot;get&quot;, &quot;get&quot;]
[[], [&quot;foo&quot;, &quot;bar&quot;, 1], [&quot;foo&quot;, 1], [&quot;foo&quot;, 3], [&quot;foo&quot;, &quot;bar2&quot;, 4], [&quot;foo&quot;, 4], [&quot;foo&quot;, 5]]
<strong>Đầu ra</strong>
[null, null, &quot;bar&quot;, &quot;bar&quot;, null, &quot;bar2&quot;, &quot;bar2&quot;]

<strong>Giải thích</strong>
TimeMap timeMap = new TimeMap();
timeMap.set(&quot;foo&quot;, &quot;bar&quot;, 1);  // store the key &quot;foo&quot; and value &quot;bar&quot; along with timestamp = 1.
timeMap.get(&quot;foo&quot;, 1);         // return &quot;bar&quot;
timeMap.get(&quot;foo&quot;, 3);         // return &quot;bar&quot;, since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is &quot;bar&quot;.
timeMap.set(&quot;foo&quot;, &quot;bar2&quot;, 4); // store the key &quot;foo&quot; and value &quot;bar2&quot; along with timestamp = 4.
timeMap.get(&quot;foo&quot;, 4);         // return &quot;bar2&quot;
timeMap.get(&quot;foo&quot;, 5);         // return &quot;bar2&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= key.length, value.length &lt;= 100</code></li>
	<li><code>key</code> và <code>value</code> chỉ gồm chữ cái tiếng Anh viết thường và chữ số.</li>
	<li><code>1 &lt;= timestamp &lt;= 10<sup>7</sup></code></li>
	<li>Tất cả timestamp <code>timestamp</code> của các lời gọi <code>set</code> đều tăng nghiêm ngặt.</li>
	<li>Có tối đa <code>2 * 10<sup>5</sup></code> lời gọi đến <code>set</code> và <code>get</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Ordered Set (hoặc tìm kiếm nhị phân)

<!-- thinking:start -->

> **Tư duy**
>
> Các timestamp của `set` tăng nghiêm ngặt, nên lịch sử của mỗi key đã được sắp xếp. `get` cần giá trị mới nhất tại thời điểm $\le$ timestamp truy vấn. Duyệt tuyến tính sẽ quá chậm với $2\times 10^5$ lời gọi. Ta lưu danh sách $(timestamp,value)$ cho từng key và tìm kiếm nhị phân vị trí upper bound.

<!-- thinking:end -->

Ta có thể dùng hash table $\textit{kvt}$ để lưu các cặp key-value, trong đó key là chuỗi $\textit{key}$ và value là một ordered set. Mỗi phần tử trong set là tuple $(\textit{timestamp}, \textit{value})$, biểu thị giá trị $\textit{value}$ tương ứng với key $\textit{key}$ tại timestamp $\textit{timestamp}$.

Khi cần truy vấn giá trị ứng với key $\textit{key}$ tại timestamp $\textit{timestamp}$, ta có thể dùng ordered set để tìm timestamp lớn nhất $\textit{timestamp}'$ thỏa mãn $\textit{timestamp}' \leq \textit{timestamp}$, rồi trả về giá trị tương ứng.

Về độ phức tạp thời gian, thao tác $\textit{set}$ có độ phức tạp $O(1)$ vì thao tác thêm vào hash table mất $O(1)$. Với thao tác $\textit{get}$, tra cứu trong hash table mất $O(1)$ và tra cứu trong ordered set mất $O(\log n)$, nên tổng độ phức tạp là $O(\log n)$. Độ phức tạp không gian là $O(n)$, trong đó $n$ là số lần gọi $\textit{set}$.

<!-- tabs:start -->

#### Python3

```python
class TimeMap:
    def __init__(self):
        self.ktv = defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        self.ktv[key].append((timestamp, value))

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.ktv:
            return ''
        tv = self.ktv[key]
        i = bisect_right(tv, (timestamp, chr(127)))
        return tv[i - 1][1] if i else ''


# Your TimeMap object will be instantiated and called as such:
# obj = TimeMap()
# obj.set(key,value,timestamp)
# param_2 = obj.get(key,timestamp)
```

#### Java

```java
class TimeMap {
    private Map<String, TreeMap<Integer, String>> ktv = new HashMap<>();

    public TimeMap() {
    }

    public void set(String key, String value, int timestamp) {
        ktv.computeIfAbsent(key, k -> new TreeMap<>()).put(timestamp, value);
    }

    public String get(String key, int timestamp) {
        if (!ktv.containsKey(key)) {
            return "";
        }
        var tv = ktv.get(key);
        Integer t = tv.floorKey(timestamp);
        return t == null ? "" : tv.get(t);
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */
```

#### C++

```cpp
class TimeMap {
public:
    TimeMap() {
    }

    void set(string key, string value, int timestamp) {
        ktv[key].emplace_back(timestamp, value);
    }

    string get(string key, int timestamp) {
        auto& pairs = ktv[key];
        pair<int, string> p = {timestamp, string({127})};
        auto i = upper_bound(pairs.begin(), pairs.end(), p);
        return i == pairs.begin() ? "" : (i - 1)->second;
    }

private:
    unordered_map<string, vector<pair<int, string>>> ktv;
};

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap* obj = new TimeMap();
 * obj->set(key,value,timestamp);
 * string param_2 = obj->get(key,timestamp);
 */
```

#### Go

```go
type TimeMap struct {
	ktv map[string][]pair
}

func Constructor() TimeMap {
	return TimeMap{map[string][]pair{}}
}

func (this *TimeMap) Set(key string, value string, timestamp int) {
	this.ktv[key] = append(this.ktv[key], pair{timestamp, value})
}

func (this *TimeMap) Get(key string, timestamp int) string {
	pairs := this.ktv[key]
	i := sort.Search(len(pairs), func(i int) bool { return pairs[i].t > timestamp })
	if i > 0 {
		return pairs[i-1].v
	}
	return ""
}

type pair struct {
	t int
	v string
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Set(key,value,timestamp);
 * param_2 := obj.Get(key,timestamp);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
