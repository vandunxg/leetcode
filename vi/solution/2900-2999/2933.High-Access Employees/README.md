---
comments: true
difficulty: Medium
rating: 1536
source: Weekly Contest 371 Q2
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [2933. High-Access Employees](https://leetcode.com/problems/high-access-employees)

[Tài liệu tiếng Trung](/solution/2900-2999/2933.High-Access%20Employees/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng chuỗi 2D <strong>đánh chỉ số từ 0</strong> <code>access_times</code> có kích thước <code>n</code>. Với mỗi <code>i</code> thỏa mãn <code>0 &lt;= i &lt;= n - 1</code>, <code>access_times[i][0]</code> biểu diễn tên của một nhân viên, còn <code>access_times[i][1]</code> biểu diễn thời điểm truy cập của nhân viên đó. Tất cả các phần tử trong <code>access_times</code> đều thuộc cùng một ngày.</p>

<p>Thời điểm truy cập được biểu diễn bằng <strong>bốn chữ số</strong> theo định dạng thời gian <strong>24 giờ</strong>, chẳng hạn <code>&quot;0800&quot;</code> hoặc <code>&quot;2250&quot;</code>.</p>

<p>Một nhân viên được xem là <strong>có tần suất truy cập cao</strong> nếu đã truy cập hệ thống <strong>từ ba lần trở lên</strong> trong khoảng thời gian <strong>một giờ</strong>.</p>

<p>Các thời điểm cách nhau đúng một giờ <strong>không</strong> được xem là thuộc cùng một khoảng thời gian một giờ. Ví dụ, <code>&quot;0815&quot;</code> và <code>&quot;0915&quot;</code> không thuộc cùng một khoảng thời gian một giờ.</p>

<p>Các thời điểm truy cập ở đầu và cuối ngày <strong>không</strong> được tính trong cùng một khoảng thời gian một giờ. Ví dụ, <code>&quot;0005&quot;</code> và <code>&quot;2350&quot;</code> không thuộc cùng một khoảng thời gian một giờ.</p>

<p>Trả về <em>một danh sách chứa tên của các nhân viên <strong>có tần suất truy cập cao</strong> theo bất kỳ thứ tự nào.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> access_times = [[&quot;a&quot;,&quot;0549&quot;],[&quot;b&quot;,&quot;0457&quot;],[&quot;a&quot;,&quot;0532&quot;],[&quot;a&quot;,&quot;0621&quot;],[&quot;b&quot;,&quot;0540&quot;]]
<strong>Đầu ra:</strong> [&quot;a&quot;]
<strong>Giải thích:</strong> &quot;a&quot; có ba thời điểm truy cập trong khoảng thời gian một giờ [05:32, 06:31], đó là 05:32, 05:49 và 06:21.
Nhưng &quot;b&quot; chỉ có nhiều nhất hai thời điểm truy cập.
Vì vậy, đáp án là [&quot;a&quot;].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> access_times = [[&quot;d&quot;,&quot;0002&quot;],[&quot;c&quot;,&quot;0808&quot;],[&quot;c&quot;,&quot;0829&quot;],[&quot;e&quot;,&quot;0215&quot;],[&quot;d&quot;,&quot;1508&quot;],[&quot;d&quot;,&quot;1444&quot;],[&quot;d&quot;,&quot;1410&quot;],[&quot;c&quot;,&quot;0809&quot;]]
<strong>Đầu ra:</strong> [&quot;c&quot;,&quot;d&quot;]
<strong>Giải thích:</strong> &quot;c&quot; có ba thời điểm truy cập trong khoảng thời gian một giờ [08:08, 09:07], đó là 08:08, 08:09 và 08:29.
&quot;d&quot; cũng có ba thời điểm truy cập trong khoảng thời gian một giờ [14:10, 15:09], đó là 14:10, 14:44 và 15:08.
Tuy nhiên, &quot;e&quot; chỉ có một thời điểm truy cập nên không thể xuất hiện trong đáp án. Do đó, đáp án cuối cùng là [&quot;c&quot;,&quot;d&quot;].</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> access_times = [[&quot;cd&quot;,&quot;1025&quot;],[&quot;ab&quot;,&quot;1025&quot;],[&quot;cd&quot;,&quot;1046&quot;],[&quot;cd&quot;,&quot;1055&quot;],[&quot;ab&quot;,&quot;1124&quot;],[&quot;ab&quot;,&quot;1120&quot;]]
<strong>Đầu ra:</strong> [&quot;ab&quot;,&quot;cd&quot;]
<strong>Giải thích:</strong> &quot;ab&quot; có ba thời điểm truy cập trong khoảng thời gian một giờ [10:25, 11:24], đó là 10:25, 11:20 và 11:24.
&quot;cd&quot; cũng có ba thời điểm truy cập trong khoảng thời gian một giờ [10:25, 11:24], đó là 10:25, 10:46 và 10:55.
Vì vậy, đáp án là [&quot;ab&quot;,&quot;cd&quot;].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= access_times.length &lt;= 100</code></li>
	<li><code>access_times[i].length == 2</code></li>
	<li><code>1 &lt;= access_times[i][0].length &lt;= 10</code></li>
	<li><code>access_times[i][0]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>access_times[i][1].length == 4</code></li>
	<li><code>access_times[i][1]</code> ở định dạng thời gian 24 giờ.</li>
	<li><code>access_times[i][1]</code> chỉ gồm các ký tự từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Truy cập nhiều nghĩa là có ít nhất ba lần truy cập trong một giờ. Sau khi nhóm theo tên, ta chuyển $HHMM$ thành số phút và sắp xếp; ba thời điểm liên tiếp nằm trong một khoảng $60$ phút khi và chỉ khi $t[i]-t[i-2] < 60$.
>
> Một hash map lưu các mốc thời gian, sau đó một lượt duyệt tuyến tính sau khi sắp xếp sẽ quyết định kết quả. Vì $n \le 100$, việc nhóm và sắp xếp có chi phí thấp.

<!-- thinking:end -->

Ta dùng một hash map $d$ để lưu tất cả thời điểm truy cập của mỗi nhân viên. Khóa là tên nhân viên, còn giá trị là một mảng số nguyên biểu diễn toàn bộ thời điểm truy cập của nhân viên đó dưới dạng số phút tính từ đầu ngày, tức 00:00.

Với mỗi nhân viên, ta sắp xếp tất cả thời điểm truy cập theo thứ tự tăng dần. Sau đó, ta duyệt toàn bộ các thời điểm truy cập của nhân viên. Nếu tồn tại ba thời điểm truy cập liên tiếp $t_1, t_2, t_3$ thỏa mãn $t_3 - t_1 < 60$, thì nhân viên đó có tần suất truy cập cao và ta thêm tên của họ vào mảng đáp án.

Cuối cùng, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số bản ghi truy cập.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findHighAccessEmployees(self, access_times: List[List[str]]) -> List[str]:
        d = defaultdict(list)
        for name, t in access_times:
            d[name].append(int(t[:2]) * 60 + int(t[2:]))
        ans = []
        for name, ts in d.items():
            ts.sort()
            if any(ts[i] - ts[i - 2] < 60 for i in range(2, len(ts))):
                ans.append(name)
        return ans
```

#### Java

```java
class Solution {
    public List<String> findHighAccessEmployees(List<List<String>> access_times) {
        Map<String, List<Integer>> d = new HashMap<>();
        for (var e : access_times) {
            String name = e.get(0), s = e.get(1);
            int t = Integer.valueOf(s.substring(0, 2)) * 60 + Integer.valueOf(s.substring(2));
            d.computeIfAbsent(name, k -> new ArrayList<>()).add(t);
        }
        List<String> ans = new ArrayList<>();
        for (var e : d.entrySet()) {
            String name = e.getKey();
            var ts = e.getValue();
            Collections.sort(ts);
            for (int i = 2; i < ts.size(); ++i) {
                if (ts.get(i) - ts.get(i - 2) < 60) {
                    ans.add(name);
                    break;
                }
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
    vector<string> findHighAccessEmployees(vector<vector<string>>& access_times) {
        unordered_map<string, vector<int>> d;
        for (auto& e : access_times) {
            auto name = e[0];
            auto s = e[1];
            int t = stoi(s.substr(0, 2)) * 60 + stoi(s.substr(2, 2));
            d[name].emplace_back(t);
        }
        vector<string> ans;
        for (auto& [name, ts] : d) {
            sort(ts.begin(), ts.end());
            for (int i = 2; i < ts.size(); ++i) {
                if (ts[i] - ts[i - 2] < 60) {
                    ans.emplace_back(name);
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findHighAccessEmployees(access_times [][]string) (ans []string) {
	d := map[string][]int{}
	for _, e := range access_times {
		name, s := e[0], e[1]
		h, _ := strconv.Atoi(s[:2])
		m, _ := strconv.Atoi(s[2:])
		t := h*60 + m
		d[name] = append(d[name], t)
	}
	for name, ts := range d {
		sort.Ints(ts)
		for i := 2; i < len(ts); i++ {
			if ts[i]-ts[i-2] < 60 {
				ans = append(ans, name)
				break
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function findHighAccessEmployees(access_times: string[][]): string[] {
    const d: Map<string, number[]> = new Map();
    for (const [name, s] of access_times) {
        const h = parseInt(s.slice(0, 2), 10);
        const m = parseInt(s.slice(2), 10);
        const t = h * 60 + m;
        if (!d.has(name)) {
            d.set(name, []);
        }
        d.get(name)!.push(t);
    }
    const ans: string[] = [];
    for (const [name, ts] of d) {
        ts.sort((a, b) => a - b);
        for (let i = 2; i < ts.length; ++i) {
            if (ts[i] - ts[i - 2] < 60) {
                ans.push(name);
                break;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
