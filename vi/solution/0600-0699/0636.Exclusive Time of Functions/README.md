---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
---

<!-- problem:start -->

# [636. Exclusive Time of Functions](https://leetcode.com/problems/exclusive-time-of-functions)

[中文文档](/solution/0600-0699/0636.Exclusive%20Time%20of%20Functions/README.md)

## Mô tả

<!-- description:start -->

<p>Trên CPU <strong>đơn luồng</strong>, ta chạy một chương trình gồm <code>n</code> function. Mỗi function có một ID duy nhất trong khoảng từ 0 đến <code>n - 1</code>.</p>

<p>Các lời gọi function được <strong>lưu trong một <a href="https://en.wikipedia.org/wiki/Call_stack">call stack</a></strong>: khi một lời gọi function bắt đầu, ID của nó được push vào stack; khi lời gọi kết thúc, ID đó được pop khỏi stack. Function có ID nằm trên cùng stack là <strong>function hiện đang được thực thi</strong>. Mỗi khi một function bắt đầu hoặc kết thúc, ta ghi log gồm ID, trạng thái bắt đầu hay kết thúc và timestamp.</p>

<p>Cho danh sách <code>logs</code>, trong đó <code>logs[i]</code> là log thứ <code>i<sup>th</sup></code> có định dạng chuỗi <code>&quot;{function_id}:{&quot;start&quot; | &quot;end&quot;}:{timestamp}&quot;</code>. Ví dụ, <code>&quot;0:start:3&quot;</code> nghĩa là lời gọi function có ID 0 <strong>bắt đầu ở đầu</strong> timestamp 3, còn <code>&quot;1:end:2&quot;</code> nghĩa là lời gọi function có ID 1 <strong>kết thúc ở cuối</strong> timestamp 2. Lưu ý, một function có thể được gọi <b>nhiều lần, kể cả đệ quy</b>.</p>

<p><strong>Exclusive time</strong> của một function là tổng thời gian thực thi của tất cả lời gọi function đó trong chương trình. Ví dụ, nếu một function được gọi hai lần, một lần chạy trong 2 đơn vị thời gian và lần còn lại chạy trong 1 đơn vị, thì <strong>exclusive time</strong> là <code>2 + 1 = 3</code>.</p>

<p>Hãy trả về <em><strong>exclusive time</strong> của mỗi function trong một mảng, trong đó giá trị tại chỉ số </em><code>i<sup>th</sup></code><em> là exclusive time của function có ID </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0636.Exclusive%20Time%20of%20Functions/images/diag1b.png" style="width: 550px; height: 239px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, logs = [&quot;0:start:0&quot;,&quot;1:start:2&quot;,&quot;1:end:5&quot;,&quot;0:end:6&quot;]
<strong>Đầu ra:</strong> [3,4]
<strong>Giải thích:</strong>
Function 0 bắt đầu ở đầu thời điểm 0, sau đó chạy trong 2 đơn vị thời gian và đến cuối thời điểm 1.
Function 1 bắt đầu ở đầu thời điểm 2, chạy trong 4 đơn vị thời gian và kết thúc ở cuối thời điểm 5.
Function 0 tiếp tục chạy từ đầu thời điểm 6 và chạy trong 1 đơn vị thời gian.
Vậy function 0 có tổng thời gian thực thi là 2 + 1 = 3 đơn vị, còn function 1 có tổng thời gian thực thi là 4 đơn vị.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, logs = [&quot;0:start:0&quot;,&quot;0:start:2&quot;,&quot;0:end:5&quot;,&quot;0:start:6&quot;,&quot;0:end:6&quot;,&quot;0:end:7&quot;]
<strong>Đầu ra:</strong> [8]
<strong>Giải thích:</strong>
Function 0 bắt đầu ở đầu thời điểm 0, chạy trong 2 đơn vị thời gian rồi gọi đệ quy chính nó.
Function 0 (lời gọi đệ quy) bắt đầu ở đầu thời điểm 2 và chạy trong 4 đơn vị thời gian.
Function 0 (lời gọi ban đầu) tiếp tục chạy rồi lập tức gọi đệ quy chính nó lần nữa.
Function 0 (lời gọi đệ quy lần thứ 2) bắt đầu ở đầu thời điểm 6 và chạy trong 1 đơn vị thời gian.
Function 0 (lời gọi ban đầu) tiếp tục chạy từ đầu thời điểm 7 và chạy trong 1 đơn vị thời gian.
Vậy function 0 có tổng thời gian thực thi là 2 + 4 + 1 + 1 = 8 đơn vị.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, logs = [&quot;0:start:0&quot;,&quot;0:start:2&quot;,&quot;0:end:5&quot;,&quot;1:start:6&quot;,&quot;1:end:6&quot;,&quot;0:end:7&quot;]
<strong>Đầu ra:</strong> [7,1]
<strong>Giải thích:</strong>
Function 0 bắt đầu ở đầu thời điểm 0, chạy trong 2 đơn vị thời gian rồi gọi đệ quy chính nó.
Function 0 (lời gọi đệ quy) bắt đầu ở đầu thời điểm 2 và chạy trong 4 đơn vị thời gian.
Function 0 (lời gọi ban đầu) tiếp tục chạy rồi lập tức gọi function 1.
Function 1 bắt đầu ở đầu thời điểm 6, chạy trong 1 đơn vị thời gian và kết thúc ở cuối thời điểm 6.
Function 0 tiếp tục chạy từ đầu thời điểm 7 và chạy trong 1 đơn vị thời gian.
Vậy function 0 có tổng thời gian thực thi là 2 + 4 + 1 = 7 đơn vị, còn function 1 có tổng thời gian thực thi là 1 đơn vị.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>2 &lt;= logs.length &lt;= 500</code></li>
	<li><code>0 &lt;= function_id &lt; n</code></li>
	<li><code>0 &lt;= timestamp &lt;= 10<sup>9</sup></code></li>
	<li>Không có hai sự kiện bắt đầu nào xảy ra cùng timestamp.</li>
	<li>Không có hai sự kiện kết thúc nào xảy ra cùng timestamp.</li>
	<li>Mỗi log <code>&quot;start&quot;</code> đều có một log <code>&quot;end&quot;</code> tương ứng cho cùng function.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các function lồng nhau và ta cần tính exclusive time. Không thể tiến từng giây trên timeline khi timestamp lớn.
>
> Stack phù hợp với cấu trúc lồng nhau: khi gặp `start`, cộng khoảng $[pre, cur)$ cho phần tử trên cùng; khi gặp `end`, cộng khoảng $[pre, cur]$ rồi đặt $pre=cur+1$. Chỉ cần duyệt logs một lần.

<!-- thinking:end -->

Ta dùng stack $\textit{stk}$ để lưu ID của các function đang thực thi. Đồng thời, ta dùng mảng $\textit{ans}$ để lưu exclusive time của từng function, ban đầu đặt thời gian của mỗi function bằng $0$. Biến $\textit{pre}$ dùng để ghi nhận timestamp trước đó.

Ta duyệt mảng log. Với mỗi mục log, trước tiên tách theo dấu hai chấm để lấy ID function $\textit{i}$, loại thao tác $\textit{op}$ và timestamp $\textit{t}$.

Nếu $\textit{op}$ là $\text{start}$, nghĩa là function $\textit{i}$ bắt đầu thực thi. Ta kiểm tra stack có rỗng không. Nếu không, cộng $\textit{cur} - \textit{pre}$ vào exclusive time của function trên cùng stack, sau đó push $\textit{i}$ vào stack và cập nhật $\textit{pre}$ thành $\textit{cur}$. Nếu $\textit{op}$ là $\text{end}$, nghĩa là function $\textit{i}$ kết thúc thực thi. Ta cộng $\textit{cur} - \textit{pre} + 1$ vào exclusive time của function trên cùng stack, sau đó pop phần tử trên cùng và cập nhật $\textit{pre}$ thành $\textit{cur} + 1$.

Cuối cùng, ta trả về mảng $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng log.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def exclusiveTime(self, n: int, logs: List[str]) -> List[int]:
        stk = []
        ans = [0] * n
        pre = 0
        for log in logs:
            i, op, t = log.split(":")
            i, cur = int(i), int(t)
            if op[0] == "s":
                if stk:
                    ans[stk[-1]] += cur - pre
                stk.append(i)
                pre = cur
            else:
                ans[stk.pop()] += cur - pre + 1
                pre = cur + 1
        return ans
```

#### Java

```java
class Solution {
    public int[] exclusiveTime(int n, List<String> logs) {
        int[] ans = new int[n];
        Deque<Integer> stk = new ArrayDeque<>();
        int pre = 0;
        for (var log : logs) {
            var parts = log.split(":");
            int i = Integer.parseInt(parts[0]);
            int cur = Integer.parseInt(parts[2]);
            if (parts[1].charAt(0) == 's') {
                if (!stk.isEmpty()) {
                    ans[stk.peek()] += cur - pre;
                }
                stk.push(i);
                pre = cur;
            } else {
                ans[stk.pop()] += cur - pre + 1;
                pre = cur + 1;
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
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        vector<int> ans(n);
        stack<int> stk;
        int pre = 0;
        for (const auto& log : logs) {
            int i, cur;
            char c[10];
            sscanf(log.c_str(), "%d:%[^:]:%d", &i, c, &cur);
            if (c[0] == 's') {
                if (stk.size()) {
                    ans[stk.top()] += cur - pre;
                }
                stk.push(i);
                pre = cur;
            } else {
                ans[stk.top()] += cur - pre + 1;
                stk.pop();
                pre = cur + 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func exclusiveTime(n int, logs []string) []int {
	ans := make([]int, n)
	stk := []int{}
	pre := 0
	for _, log := range logs {
		parts := strings.Split(log, ":")
		i, _ := strconv.Atoi(parts[0])
		cur, _ := strconv.Atoi(parts[2])
		if parts[1][0] == 's' {
			if len(stk) > 0 {
				ans[stk[len(stk)-1]] += cur - pre
			}
			stk = append(stk, i)
			pre = cur
		} else {
			ans[stk[len(stk)-1]] += cur - pre + 1
			stk = stk[:len(stk)-1]
			pre = cur + 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function exclusiveTime(n: number, logs: string[]): number[] {
    const ans: number[] = Array(n).fill(0);
    let pre = 0;
    const stk: number[] = [];
    for (const log of logs) {
        const [i, op, cur] = log.split(':');
        if (op[0] === 's') {
            if (stk.length) {
                ans[stk.at(-1)!] += +cur - pre;
            }
            stk.push(+i);
            pre = +cur;
        } else {
            ans[stk.pop()!] += +cur - pre + 1;
            pre = +cur + 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
