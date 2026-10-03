---
comments: true
difficulty: Medium
rating: 1636
source: Biweekly Contest 94 Q2
tags:
    - Array
    - Hash Table
    - String
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2512. Reward Top K Students](https://leetcode.com/problems/reward-top-k-students)

[中文文档](/solution/2500-2599/2512.Reward%20Top%20K%20Students/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>positive_feedback</code> và <code>negative_feedback</code>, lần lượt chứa các từ biểu thị phản hồi tích cực và tiêu cực. Lưu ý rằng <strong>không có</strong> từ nào vừa tích cực vừa tiêu cực.</p>

<p>Ban đầu, mỗi học sinh có <code>0</code> điểm. Mỗi từ tích cực trong một báo cáo phản hồi <strong>tăng</strong> điểm của học sinh lên <code>3</code>, còn mỗi từ tiêu cực <strong>giảm</strong> điểm đi <code>1</code>.</p>

<p>Bạn được cho <code>n</code> báo cáo phản hồi, biểu diễn bởi mảng chuỗi <code>report</code> được đánh chỉ số <strong>0-based</strong> và mảng số nguyên <code>student_id</code> cũng được đánh chỉ số <strong>0-based</strong>, trong đó <code>student_id[i]</code> là ID của học sinh nhận báo cáo phản hồi <code>report[i]</code>. ID của mỗi học sinh là <strong>duy nhất</strong>.</p>

<p>Cho số nguyên <code>k</code>, <em>hãy trả về </em><code>k</code><em> học sinh đứng đầu sau khi xếp hạng theo thứ tự <strong>không tăng</strong> của điểm số</em>. Nếu có nhiều học sinh có cùng điểm, học sinh có ID nhỏ hơn được xếp hạng cao hơn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> positive_feedback = [&quot;smart&quot;,&quot;brilliant&quot;,&quot;studious&quot;], negative_feedback = [&quot;not&quot;], report = [&quot;this student is studious&quot;,&quot;the student is smart&quot;], student_id = [1,2], k = 2
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong>
Cả hai học sinh đều có 1 phản hồi tích cực và 3 điểm, nhưng học sinh 1 có ID nhỏ hơn nên được xếp hạng cao hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> positive_feedback = [&quot;smart&quot;,&quot;brilliant&quot;,&quot;studious&quot;], negative_feedback = [&quot;not&quot;], report = [&quot;this student is not studious&quot;,&quot;the student is smart&quot;], student_id = [1,2], k = 2
<strong>Đầu ra:</strong> [2,1]
<strong>Giải thích:</strong>
- Học sinh có ID 1 có 1 phản hồi tích cực và 1 phản hồi tiêu cực, nên có 3-1=2 điểm.
- Học sinh có ID 2 có 1 phản hồi tích cực, nên có 3 điểm.
Vì học sinh 2 có nhiều điểm hơn nên trả về [2,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= positive_feedback.length, negative_feedback.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= positive_feedback[i].length, negative_feedback[j].length &lt;= 100</code></li>
	<li>Cả <code>positive_feedback[i]</code> và <code>negative_feedback[j]</code> đều chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Không có từ nào xuất hiện trong cả <code>positive_feedback</code> và <code>negative_feedback</code>.</li>
	<li><code>n == report.length == student_id.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>report[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường và dấu cách <code>&#39; &#39;</code>.</li>
	<li>Có một dấu cách duy nhất giữa hai từ liên tiếp trong <code>report[i]</code>.</li>
	<li><code>1 &lt;= report[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= student_id[i] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả các giá trị của <code>student_id[i]</code> là <strong>duy nhất</strong>.</li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Điểm của một học sinh tăng $+3$ với mỗi từ tích cực và giảm $-1$ với mỗi từ tiêu cực trong báo cáo; $k$ học sinh đứng đầu là những học sinh có điểm cao hơn, sau đó là ID nhỏ hơn. Việc tìm kiếm tuyến tính trong các danh sách từ ở mỗi token sẽ nhân kích thước danh sách với độ dài báo cáo.
>
> Lưu hai danh sách từ vào các set để phân loại mỗi token trong $O(1)$. Thu thập $(\textit{score},\textit{id})$, sắp xếp theo $(-\textit{score},\textit{id})$, rồi lấy $k$ ID đầu tiên.

<!-- thinking:end -->

Chúng ta có thể lưu các từ tích cực trong một hash table $ps$ và các từ tiêu cực trong một hash table $ns$.

Sau đó, ta duyệt qua $report$ và với mỗi học sinh, lưu điểm của họ vào một mảng $arr$, trong đó mỗi phần tử là một tuple $(t, sid)$, với $t$ là điểm của học sinh và $sid$ là ID của học sinh.

Cuối cùng, ta sắp xếp mảng $arr$ theo thứ tự giảm dần của điểm; nếu điểm bằng nhau thì sắp xếp theo ID tăng dần. Sau đó, ta lấy ID của $k$ học sinh đứng đầu.

Độ phức tạp thời gian là $O(n \times \log n + (|ps| + |ns| + n) \times |s|)$, và độ phức tạp không gian là $O((|ps|+|ns|) \times |s| + n)$. Ở đây, $n$ là số học sinh, $|ps|$ và $|ns|$ lần lượt là số từ tích cực và tiêu cực, còn $|s|$ là độ dài trung bình của một từ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def topStudents(
        self,
        positive_feedback: List[str],
        negative_feedback: List[str],
        report: List[str],
        student_id: List[int],
        k: int,
    ) -> List[int]:
        ps = set(positive_feedback)
        ns = set(negative_feedback)
        arr = []
        for sid, r in zip(student_id, report):
            t = 0
            for w in r.split():
                if w in ps:
                    t += 3
                elif w in ns:
                    t -= 1
            arr.append((t, sid))
        arr.sort(key=lambda x: (-x[0], x[1]))
        return [v[1] for v in arr[:k]]
```

#### Java

```java
class Solution {
    public List<Integer> topStudents(String[] positive_feedback, String[] negative_feedback,
        String[] report, int[] student_id, int k) {
        Set<String> ps = new HashSet<>();
        Set<String> ns = new HashSet<>();
        for (var s : positive_feedback) {
            ps.add(s);
        }
        for (var s : negative_feedback) {
            ns.add(s);
        }
        int n = report.length;
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            int sid = student_id[i];
            int t = 0;
            for (var s : report[i].split(" ")) {
                if (ps.contains(s)) {
                    t += 3;
                } else if (ns.contains(s)) {
                    t -= 1;
                }
            }
            arr[i] = new int[] {t, sid};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < k; ++i) {
            ans.add(arr[i][1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> topStudents(vector<string>& positive_feedback, vector<string>& negative_feedback, vector<string>& report, vector<int>& student_id, int k) {
        unordered_set<string> ps(positive_feedback.begin(), positive_feedback.end());
        unordered_set<string> ns(negative_feedback.begin(), negative_feedback.end());
        vector<pair<int, int>> arr;
        int n = report.size();
        for (int i = 0; i < n; ++i) {
            int sid = student_id[i];
            vector<string> ws = split(report[i], ' ');
            int t = 0;
            for (auto& w : ws) {
                if (ps.count(w)) {
                    t += 3;
                } else if (ns.count(w)) {
                    t -= 1;
                }
            }
            arr.push_back({-t, sid});
        }
        sort(arr.begin(), arr.end());
        vector<int> ans;
        for (int i = 0; i < k; ++i) {
            ans.emplace_back(arr[i].second);
        }
        return ans;
    }

    vector<string> split(string& s, char delim) {
        stringstream ss(s);
        string item;
        vector<string> res;
        while (getline(ss, item, delim)) {
            res.emplace_back(item);
        }
        return res;
    }
};
```

#### Go

```go
func topStudents(positive_feedback []string, negative_feedback []string, report []string, student_id []int, k int) (ans []int) {
	ps := map[string]bool{}
	ns := map[string]bool{}
	for _, s := range positive_feedback {
		ps[s] = true
	}
	for _, s := range negative_feedback {
		ns[s] = true
	}
	arr := [][2]int{}
	for i, sid := range student_id {
		t := 0
		for _, w := range strings.Split(report[i], " ") {
			if ps[w] {
				t += 3
			} else if ns[w] {
				t -= 1
			}
		}
		arr = append(arr, [2]int{t, sid})
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i][0] > arr[j][0] || (arr[i][0] == arr[j][0] && arr[i][1] < arr[j][1]) })
	for _, v := range arr[:k] {
		ans = append(ans, v[1])
	}
	return
}
```

#### TypeScript

```ts
function topStudents(
    positive_feedback: string[],
    negative_feedback: string[],
    report: string[],
    student_id: number[],
    k: number,
): number[] {
    const n = student_id.length;
    const map = new Map<number, number>();
    const ps = new Set(positive_feedback);
    const ns = new Set(negative_feedback);
    for (let i = 0; i < n; i++) {
        map.set(
            student_id[i],
            report[i].split(' ').reduce((r, s) => {
                if (ps.has(s)) {
                    return r + 3;
                }
                if (ns.has(s)) {
                    return r - 1;
                }
                return r;
            }, 0),
        );
    }
    return [...map.entries()]
        .sort((a, b) => {
            if (a[1] === b[1]) {
                return a[0] - b[0];
            }
            return b[1] - a[1];
        })
        .map(v => v[0])
        .slice(0, k);
}
```

#### Rust

```rust
use std::collections::{HashMap, HashSet};
impl Solution {
    pub fn top_students(
        positive_feedback: Vec<String>,
        negative_feedback: Vec<String>,
        report: Vec<String>,
        student_id: Vec<i32>,
        k: i32,
    ) -> Vec<i32> {
        let n = student_id.len();
        let ps = positive_feedback.iter().collect::<HashSet<&String>>();
        let ns = negative_feedback.iter().collect::<HashSet<&String>>();
        let mut map = HashMap::new();
        for i in 0..n {
            let id = student_id[i];
            let mut count = 0;
            for s in report[i].split(' ') {
                let s = &s.to_string();
                if ps.contains(s) {
                    count += 3;
                } else if ns.contains(s) {
                    count -= 1;
                }
            }
            map.insert(id, count);
        }
        let mut t = map.into_iter().collect::<Vec<(i32, i32)>>();
        t.sort_by(|a, b| {
            if a.1 == b.1 {
                return a.0.cmp(&b.0);
            }
            b.1.cmp(&a.1)
        });
        t.iter().map(|v| v.0).collect::<Vec<i32>>()[0..k as usize].to_vec()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
