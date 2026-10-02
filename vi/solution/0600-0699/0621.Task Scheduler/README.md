---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [621. Task Scheduler](https://leetcode.com/problems/task-scheduler)

[中文文档](/solution/0600-0699/0621.Task%20Scheduler/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>tasks</code> gồm các tác vụ CPU, mỗi tác vụ được gán một chữ cái từ A đến Z, và số <code>n</code>. Mỗi khoảng thời gian của CPU có thể để trống hoặc thực hiện một tác vụ. Có thể xử lý tác vụ theo bất kỳ thứ tự nào, nhưng giữa hai tác vụ cùng nhãn phải có khoảng cách <strong>ít nhất</strong> <code>n</code> khoảng thời gian.</p>

<p>Hãy trả về số khoảng thời gian CPU <strong>ít nhất</strong> cần để hoàn thành tất cả tác vụ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">tasks = [&quot;A&quot;,&quot;A&quot;,&quot;A&quot;,&quot;B&quot;,&quot;B&quot;,&quot;B&quot;], n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
font-family: Menlo,sans-serif;
font-size: 0.85rem;
">8</span></p>

<p><strong>Giải thích:</strong> Một lịch thực hiện khả dĩ là: A -&gt; B -&gt; idle -&gt; A -&gt; B -&gt; idle -&gt; A -&gt; B.</p>

<p>Sau khi hoàn thành tác vụ A, phải chờ hai khoảng thời gian trước khi thực hiện A lần nữa. Tương tự với tác vụ B. Ở khoảng thời gian thứ 3, không thể thực hiện A hay B nên CPU nghỉ. Đến khoảng thời gian thứ 4, đã qua hai khoảng nên có thể thực hiện A lại.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">tasks = [&quot;A&quot;,&quot;C&quot;,&quot;A&quot;,&quot;B&quot;,&quot;D&quot;,&quot;B&quot;], n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">6</span></p>

<p><strong>Giải thích:</strong> Một lịch thực hiện khả dĩ là: A -&gt; B -&gt; C -&gt; D -&gt; A -&gt; B.</p>

<p>Với khoảng nghỉ là 1, có thể lặp lại một tác vụ sau khi thực hiện một tác vụ khác.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">tasks = [&quot;A&quot;,&quot;A&quot;,&quot;A&quot;, &quot;B&quot;,&quot;B&quot;,&quot;B&quot;], n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">10</span></p>

<p><strong>Giải thích:</strong> Một lịch thực hiện khả dĩ là: A -&gt; B -&gt; idle -&gt; idle -&gt; A -&gt; B -&gt; idle -&gt; idle -&gt; A -&gt; B.</p>

<p>Chỉ có hai loại tác vụ là A và B, và các lần lặp phải cách nhau 3 khoảng thời gian. Vì vậy, giữa các lần lặp của chúng, CPU phải nghỉ hai lần.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tasks.length &lt;= 10<sup>4</sup></code></li>
	<li><code>tasks[i]</code> is an uppercase English letter.</li>
	<li><code>0 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tác vụ giống nhau phải cách nhau ít nhất $n$ khoảng. Khi đã biết tác vụ tạo ra nút thắt, không cần mô phỏng toàn bộ lịch thực hiện.
>
> Tác vụ có tần suất cao nhất là $x$ tạo thành $(x-1)$ khoảng đầy đủ, cộng thêm $s$ tác vụ có cùng tần suất ở cuối. Nếu tổng số tác vụ lớn hơn độ dài khung lịch này, các khoảng nghỉ sẽ được lấp đầy; do đó đáp án là $\max(m, (x-1)(n+1)+s)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        cnt = Counter(tasks)
        x = max(cnt.values())
        s = sum(v == x for v in cnt.values())
        return max(len(tasks), (x - 1) * (n + 1) + s)
```

#### Java

```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] cnt = new int[26];
        int x = 0;
        for (char c : tasks) {
            c -= 'A';
            ++cnt[c];
            x = Math.max(x, cnt[c]);
        }
        int s = 0;
        for (int v : cnt) {
            if (v == x) {
                ++s;
            }
        }
        return Math.max(tasks.length, (x - 1) * (n + 1) + s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> cnt(26);
        int x = 0;
        for (char c : tasks) {
            c -= 'A';
            ++cnt[c];
            x = max(x, cnt[c]);
        }
        int s = 0;
        for (int v : cnt) {
            s += v == x;
        }
        return max((int) tasks.size(), (x - 1) * (n + 1) + s);
    }
};
```

#### Go

```go
func leastInterval(tasks []byte, n int) int {
	cnt := make([]int, 26)
	x := 0
	for _, c := range tasks {
		c -= 'A'
		cnt[c]++
		x = max(x, cnt[c])
	}
	s := 0
	for _, v := range cnt {
		if v == x {
			s++
		}
	}
	return max(len(tasks), (x-1)*(n+1)+s)
}
```

#### C#

```cs
public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        int[] cnt = new int[26];
        int x = 0;
        foreach (char c in tasks) {
            cnt[c - 'A']++;
            x = Math.Max(x, cnt[c - 'A']);
        }
        int s = 0;
        foreach (int v in cnt) {
            s = v == x ? s + 1 : s;
        }
        return Math.Max(tasks.Length, (x - 1) * (n + 1) + s);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
