---
comments: true
difficulty: Easy
rating: 1262
source: Biweekly Contest 182 Q1
---

<!-- problem:start -->

# [3921. Score Validator](https://leetcode.com/problems/score-validator)

[中文文档](/solution/3900-3999/3921.Score%20Validator/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>events</code>.</p>

<p>Ban đầu, <code>score = 0</code> và <code>counter = 0</code>. Mỗi phần tử trong <code>events</code> thuộc một trong các loại sau:</p>

<ul>
	<li><code>&quot;0&quot;</code>, <code>&quot;1&quot;</code>, <code>&quot;2&quot;</code>, <code>&quot;3&quot;</code>, <code>&quot;4&quot;</code>, <code>&quot;6&quot;</code>: Cộng giá trị đó vào tổng điểm.</li>
	<li><code>&quot;W&quot;</code>: Tăng counter lên 1. Không cộng điểm.</li>
	<li><code>&quot;WD&quot;</code>: Cộng 1 vào tổng điểm.</li>
	<li><code>&quot;NB&quot;</code>: Cộng 1 vào tổng điểm.</li>
</ul>

<p>Xử lý mảng từ trái sang phải. Dừng xử lý khi một trong hai điều kiện sau xảy ra:</p>

<ul>
	<li>Đã xử lý tất cả phần tử trong <code>events</code>, hoặc</li>
	<li>counter đạt 10.</li>
</ul>

<p>Trả về một mảng số nguyên <code>[score, counter]</code>, trong đó:</p>

<ul>
	<li><code>score</code> là tổng điểm cuối cùng.</li>
	<li><code>counter</code> là giá trị cuối cùng của bộ đếm.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">events = [&quot;1&quot;,&quot;4&quot;,&quot;W&quot;,&quot;6&quot;,&quot;WD&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[12,1]</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Sự kiện</th>
			<th>Điểm</th>
			<th>Bộ đếm</th>
		</tr>
		<tr>
			<td><code>&quot;1&quot;</code></td>
			<td>1</td>
			<td>0</td>
		</tr>
		<tr>
			<td><code>&quot;4&quot;</code></td>
			<td>5</td>
			<td>0</td>
		</tr>
		<tr>
			<td><code>&quot;W&quot;</code></td>
			<td>5</td>
			<td>1</td>
		</tr>
		<tr>
			<td><code>&quot;6&quot;</code></td>
			<td>11</td>
			<td>1</td>
		</tr>
		<tr>
			<td><code>&quot;WD&quot;</code></td>
			<td>12</td>
			<td>1</td>
		</tr>
	</tbody>
</table>

<p>Kết quả cuối cùng: <code>[12, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">events = [&quot;WD&quot;,&quot;NB&quot;,&quot;0&quot;,&quot;4&quot;,&quot;4&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[10,0]</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Sự kiện</th>
			<th>Điểm</th>
			<th>Bộ đếm</th>
		</tr>
		<tr>
			<td><code>&quot;WD&quot;</code></td>
			<td>1</td>
			<td>0</td>
		</tr>
		<tr>
			<td><code>&quot;NB&quot;</code></td>
			<td>2</td>
			<td>0</td>
		</tr>
		<tr>
			<td><code>&quot;0&quot;</code></td>
			<td>2</td>
			<td>0</td>
		</tr>
		<tr>
			<td><code>&quot;4&quot;</code></td>
			<td>6</td>
			<td>0</td>
		</tr>
		<tr>
			<td><code>&quot;4&quot;</code></td>
			<td>10</td>
			<td>0</td>
		</tr>
	</tbody>
</table>

<p>Kết quả cuối cùng: <code>[10, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">events = [&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;,&quot;W&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,10]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau 10 lần xuất hiện của <code>&quot;W&quot;</code>, counter đạt 10 nên quá trình xử lý dừng lại. Các sự kiện còn lại bị bỏ qua.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= events.length &lt;= 1000</code></li>
	<li><code>events[i]</code> là một trong các giá trị <code>&quot;0&quot;</code>, <code>&quot;1&quot;</code>, <code>&quot;2&quot;</code>, <code>&quot;3&quot;</code>, <code>&quot;4&quot;</code>, <code>&quot;6&quot;</code>, <code>&quot;W&quot;</code>, <code>&quot;WD&quot;</code> hoặc <code>&quot;NB&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có một vài loại sự kiện và độ dài danh sách không vượt quá $1000$, nên mô phỏng trực tiếp là đủ.
>
> Chuỗi chữ số được cộng vào điểm; `W` tăng bộ đếm và dừng khi đạt $10$; `WD` và `NB` mỗi loại cộng một điểm. Chỉ cần duyệt một lượt là thu được cặp kết quả cuối cùng.
>
> $\textit{isdigit}$ phân biệt các sự kiện tính điểm; các nhánh còn lại xử lý bộ đếm wicket và các điểm cộng thêm.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình được mô tả trong đề bài để tính điểm và giá trị bộ đếm cuối cùng.

Đầu tiên, ta khởi tạo hai biến $\textit{score}$ và $\textit{counter}$, lần lượt biểu diễn tổng điểm hiện tại và giá trị bộ đếm. Sau đó, ta duyệt qua từng sự kiện trong mảng $\textit{events}$ và cập nhật $\textit{score}$, $\textit{counter}$ dựa trên loại sự kiện:

- Nếu sự kiện là một chuỗi số, ta chuyển nó thành số nguyên và cộng vào $\textit{score}$.
- Nếu sự kiện là chuỗi `"W"`, ta tăng $\textit{counter}$ lên 1 và kiểm tra xem nó đã đạt 10 hay chưa; nếu đã đạt, ta dừng xử lý.
- Nếu không (sự kiện là `"WD"` hoặc `"NB"`), ta cộng 1 vào $\textit{score}$.

Sau khi xử lý tất cả sự kiện hoặc khi bộ đếm đạt 10, ta trả về một mảng chứa các giá trị cuối cùng của $\textit{score}$ và $\textit{counter}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{events}$. Độ phức tạp không gian là $O(1)$ vì ta chỉ sử dụng một lượng bộ nhớ bổ sung không đổi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def scoreValidator(self, events: list[str]) -> list[int]:
        score = counter = 0
        for event in events:
            if event.isdigit():
                score += int(event)
            elif event == "W":
                counter += 1
                if counter == 10:
                    break
            else:
                score += 1
        return [score, counter]
```

#### Java

```java
class Solution {
    public int[] scoreValidator(String[] events) {
        int score = 0;
        int counter = 0;
        for (String event : events) {
            if (event.matches("\\d+")) {
                score += Integer.parseInt(event);
            } else if (event.equals("W")) {
                if (++counter == 10) {
                    break;
                }
            } else {
                score++;
            }
        }
        return new int[] {score, counter};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> scoreValidator(vector<string>& events) {
        int score = 0;
        int counter = 0;
        for (string event : events) {
            if (isdigit(event[0])) {
                score += stoi(event);
            } else if (event == "W") {
                if (++counter == 10) {
                    break;
                }
            } else {
                score++;
            }
        }
        return {score, counter};
    }
};
```

#### Go

```go
func scoreValidator(events []string) []int {
	score := 0
	counter := 0
	for _, event := range events {
		if num, err := strconv.Atoi(event); err == nil {
			score += num
		} else if event == "W" {
			counter++
			if counter == 10 {
				break
			}
		} else {
			score++
		}
	}
	return []int{score, counter}
}
```

#### TypeScript

```ts
function scoreValidator(events: string[]): number[] {
    let score = 0;
    let counter = 0;
    for (const event of events) {
        if (/^\d+$/.test(event)) {
            score += parseInt(event);
        } else if (event === 'W') {
            counter++;
            if (counter === 10) {
                break;
            }
        } else {
            score++;
        }
    }
    return [score, counter];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
