---
comments: true
difficulty: Medium
tags:
    - Design
    - Hash Table
    - String
    - Ordered Set
---

<!-- problem:start -->

# [635. Design Log Storage System 🔒](https://leetcode.com/problems/design-log-storage-system)

[中文文档](/solution/0600-0699/0635.Design%20Log%20Storage%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số log, mỗi log chứa ID duy nhất và timestamp. Timestamp là chuỗi có định dạng <code>Year:Month:Day:Hour:Minute:Second</code>, ví dụ <code>2017:01:01:23:59:59</code>. Tất cả các thành phần đều là số thập phân có thêm số 0 ở đầu nếu cần.</p>

<p>Triển khai class <code>LogSystem</code>:</p>

<ul>
	<li><code>LogSystem()</code> khởi tạo đối tượng <code>LogSystem</code>.</li>
	<li><code>void put(int id, string timestamp)</code> lưu log <code>(id, timestamp)</code> đã cho vào hệ thống lưu trữ.</li>
	<li><code>int[] retrieve(string start, string end, string granularity)</code> trả về ID của các log có timestamp nằm trong khoảng từ <code>start</code> đến <code>end</code>, bao gồm cả hai đầu mút. <code>start</code> và <code>end</code> có cùng định dạng với <code>timestamp</code>, còn <code>granularity</code> xác định độ chi tiết cần xét của khoảng (ví dụ chính xác đến <code>Day</code>, <code>Minute</code>, v.v.). Chẳng hạn, <code>start = &quot;2017:01:01:23:59:59&quot;</code>, <code>end = &quot;2017:01:02:23:59:59&quot;</code> và <code>granularity = &quot;Day&quot;</code> nghĩa là cần tìm các log trong khoảng từ <strong>ngày 1 tháng 1 năm 2017</strong> đến <strong>ngày 2 tháng 1 năm 2017</strong>, tính cả hai ngày; có thể bỏ qua <code>Hour</code>, <code>Minute</code> và <code>Second</code> của mỗi log.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;LogSystem&quot;, &quot;put&quot;, &quot;put&quot;, &quot;put&quot;, &quot;retrieve&quot;, &quot;retrieve&quot;]
[[], [1, &quot;2017:01:01:23:59:59&quot;], [2, &quot;2017:01:01:22:59:59&quot;], [3, &quot;2016:01:01:00:00:00&quot;], [&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Year&quot;], [&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Hour&quot;]]
<strong>Đầu ra</strong>
[null, null, null, null, [3, 2, 1], [2, 1]]

<strong>Giải thích</strong>
LogSystem logSystem = new LogSystem();
logSystem.put(1, &quot;2017:01:01:23:59:59&quot;);
logSystem.put(2, &quot;2017:01:01:22:59:59&quot;);
logSystem.put(3, &quot;2016:01:01:00:00:00&quot;);

// return [3,2,1], because you need to return all logs between 2016 and 2017.
logSystem.retrieve(&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Year&quot;);

// return [2,1], because you need to return all logs between Jan. 1, 2016 01:XX:XX and Jan. 1, 2017 23:XX:XX.
// Log 3 is not returned because Jan. 1, 2016 00:00:00 comes before the start of the range.
logSystem.retrieve(&quot;2016:01:01:01:01:01&quot;, &quot;2017:01:01:23:00:00&quot;, &quot;Hour&quot;);
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= id &lt;= 500</code></li>
	<li><code>2000 &lt;= Year &lt;= 2017</code></li>
	<li><code>1 &lt;= Month &lt;= 12</code></li>
	<li><code>1 &lt;= Day &lt;= 31</code></li>
	<li><code>0 &lt;= Hour &lt;= 23</code></li>
	<li><code>0 &lt;= Minute, Second &lt;= 59</code></li>
	<li><code>granularity</code> là một trong các giá trị <code>[&quot;Year&quot;, &quot;Month&quot;, &quot;Day&quot;, &quot;Hour&quot;, &quot;Minute&quot;, &quot;Second&quot;]</code>.</li>
	<li>Số lần gọi <code>put</code> và <code>retrieve</code> tối đa là <code>500</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Truy vấn lọc timestamp theo độ chi tiết được chọn. Chuyển từng thành phần thành số nguyên rồi cắt bớt các trường sẽ rườm rà.
>
> Timestamp có độ dài cố định và thứ tự từ điển cũng chính là thứ tự thời gian. Cắt chuỗi đến độ dài tương ứng với granularity rồi so sánh `start[:i] <= ts[:i] <= end[:i]`. `put` chỉ cần thêm log vào danh sách.

<!-- thinking:end -->

Lưu `id` và `timestamp` của log dưới dạng tuple trong một mảng. Trong method `retrieve()`, cắt `start` và `end` đến phần tương ứng với `granularity`, rồi duyệt mảng và thêm các `id` thỏa điều kiện vào mảng kết quả.

Về độ phức tạp thời gian, method `put()` có độ phức tạp $O(1)$, còn method `retrieve()` có độ phức tạp $O(n)$, trong đó $n$ là số phần tử trong mảng.

<!-- tabs:start -->

#### Python3

```python
class LogSystem:
    def __init__(self):
        self.logs = []
        self.d = {
            "Year": 4,
            "Month": 7,
            "Day": 10,
            "Hour": 13,
            "Minute": 16,
            "Second": 19,
        }

    def put(self, id: int, timestamp: str) -> None:
        self.logs.append((id, timestamp))

    def retrieve(self, start: str, end: str, granularity: str) -> List[int]:
        i = self.d[granularity]
        return [id for id, ts in self.logs if start[:i] <= ts[:i] <= end[:i]]


# Your LogSystem object will be instantiated and called as such:
# obj = LogSystem()
# obj.put(id,timestamp)
# param_2 = obj.retrieve(start,end,granularity)
```

#### Java

```java
class LogSystem {
    private List<Log> logs = new ArrayList<>();
    private Map<String, Integer> d = new HashMap<>();

    public LogSystem() {
        d.put("Year", 4);
        d.put("Month", 7);
        d.put("Day", 10);
        d.put("Hour", 13);
        d.put("Minute", 16);
        d.put("Second", 19);
    }

    public void put(int id, String timestamp) {
        logs.add(new Log(id, timestamp));
    }

    public List<Integer> retrieve(String start, String end, String granularity) {
        List<Integer> ans = new ArrayList<>();
        int i = d.get(granularity);
        String s = start.substring(0, i);
        String e = end.substring(0, i);
        for (var log : logs) {
            String t = log.ts.substring(0, i);
            if (s.compareTo(t) <= 0 && t.compareTo(e) <= 0) {
                ans.add(log.id);
            }
        }
        return ans;
    }
}

class Log {
    int id;
    String ts;

    Log(int id, String ts) {
        this.id = id;
        this.ts = ts;
    }
}

/**
 * Your LogSystem object will be instantiated and called as such:
 * LogSystem obj = new LogSystem();
 * obj.put(id,timestamp);
 * List<Integer> param_2 = obj.retrieve(start,end,granularity);
 */
```

#### C++

```cpp
class LogSystem {
public:
    LogSystem() {
        d["Year"] = 4;
        d["Month"] = 7;
        d["Day"] = 10;
        d["Hour"] = 13;
        d["Minute"] = 16;
        d["Second"] = 19;
    }

    void put(int id, string timestamp) {
        logs.push_back({id, timestamp});
    }

    vector<int> retrieve(string start, string end, string granularity) {
        vector<int> ans;
        int i = d[granularity];
        auto s = start.substr(0, i);
        auto e = end.substr(0, i);
        for (auto& [id, ts] : logs) {
            auto t = ts.substr(0, i);
            if (s <= t && t <= e) {
                ans.emplace_back(id);
            }
        }
        return ans;
    }

private:
    vector<pair<int, string>> logs;
    unordered_map<string, int> d;
};

/**
 * Your LogSystem object will be instantiated and called as such:
 * LogSystem* obj = new LogSystem();
 * obj->put(id,timestamp);
 * vector<int> param_2 = obj->retrieve(start,end,granularity);
 */
```

#### Go

```go
type LogSystem struct {
	logs []pair
	d    map[string]int
}

func Constructor() LogSystem {
	d := map[string]int{
		"Year":   4,
		"Month":  7,
		"Day":    10,
		"Hour":   13,
		"Minute": 16,
		"Second": 19,
	}
	return LogSystem{[]pair{}, d}
}

func (this *LogSystem) Put(id int, timestamp string) {
	this.logs = append(this.logs, pair{id, timestamp})
}

func (this *LogSystem) Retrieve(start string, end string, granularity string) (ans []int) {
	i := this.d[granularity]
	s, e := start[:i], end[:i]
	for _, log := range this.logs {
		t := log.ts[:i]
		if s <= t && t <= e {
			ans = append(ans, log.id)
		}
	}
	return
}

type pair struct {
	id int
	ts string
}

/**
 * Your LogSystem object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Put(id,timestamp);
 * param_2 := obj.Retrieve(start,end,granularity);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
