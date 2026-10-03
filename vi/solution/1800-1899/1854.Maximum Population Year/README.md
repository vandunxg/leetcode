---
comments: true
difficulty: Easy
rating: 1370
source: Weekly Contest 240 Q1
tags:
    - Array
    - Counting
    - Prefix Sum
---

<!-- problem:start -->

# [1854. Maximum Population Year](https://leetcode.com/problems/maximum-population-year)

[中文文档](/solution/1800-1899/1854.Maximum%20Population%20Year/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>logs</code>, trong đó mỗi <code>logs[i] = [birth<sub>i</sub>, death<sub>i</sub>]</code> cho biết năm sinh và năm mất của người thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Dân số</strong> của một năm <code>x</code> là số người còn sống trong năm đó. Người thứ <code>i<sup>th</sup></code> được tính vào dân số của năm <code>x</code> nếu <code>x</code> nằm trong đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[birth<sub>i</sub>, death<sub>i</sub> - 1]</code>. Lưu ý rằng người đó <strong>không</strong> được tính trong năm họ qua đời.</p>

<p>Trả về <em>năm <strong>sớm nhất</strong> có <strong>dân số lớn nhất</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> logs = [[1993,1999],[2000,2010]]
<strong>Đầu ra:</strong> 1993
<strong>Giải thích:</strong> Dân số lớn nhất là 1, và 1993 là năm sớm nhất có dân số này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> logs = [[1950,1961],[1960,1971],[1970,1981]]
<strong>Đầu ra:</strong> 1960
<strong>Giải thích:</strong>
Dân số lớn nhất là 2, và giá trị này xuất hiện trong các năm 1960 và 1970.
Năm sớm hơn trong hai năm đó là 1960.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= logs.length &lt;= 100</code></li>
	<li><code>1950 &lt;= birth<sub>i</sub> &lt; death<sub>i</sub> &lt;= 2050</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần năm sớm nhất có dân số lớn nhất; các năm nằm trong $[1950,2050]$. Việc đếm lại mọi người cho từng năm sẽ lặp lại nhiều thao tác.
>
> Miền giá trị nhỏ, nên mảng hiệu sẽ cộng $1$ tại năm sinh và trừ $1$ tại năm mất. Quét tổng tiền tố cho ta dân số của từng năm; giá trị lớn nhất đầu tiên, sau khi cộng lại $1950$, là đáp án.

<!-- thinking:end -->

Ta nhận thấy phạm vi các năm là $[1950,..2050]$. Vì vậy, ta có thể ánh xạ các năm này vào một mảng $d$ có độ dài $101$, trong đó chỉ số của mảng biểu diễn giá trị của năm trừ đi $1950$.

Tiếp theo, ta duyệt $logs$. Với mỗi người, ta tăng $d[birth_i - 1950]$ lên $1$ và giảm $d[death_i - 1950]$ đi $1$. Cuối cùng, ta duyệt mảng $d$, tìm giá trị lớn nhất của tổng tiền tố, đó là năm có dân số lớn nhất, rồi cộng $1950$ để thu được đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài của mảng $logs$, còn $C$ là kích thước miền giá trị các năm, tức là $2050 - 1950 + 1 = 101$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumPopulation(self, logs: List[List[int]]) -> int:
        d = [0] * 101
        offset = 1950
        for a, b in logs:
            a, b = a - offset, b - offset
            d[a] += 1
            d[b] -= 1
        s = mx = j = 0
        for i, x in enumerate(d):
            s += x
            if mx < s:
                mx, j = s, i
        return j + offset
```

#### Java

```java
class Solution {
    public int maximumPopulation(int[][] logs) {
        int[] d = new int[101];
        final int offset = 1950;
        for (var log : logs) {
            int a = log[0] - offset;
            int b = log[1] - offset;
            ++d[a];
            --d[b];
        }
        int s = 0, mx = 0;
        int j = 0;
        for (int i = 0; i < d.length; ++i) {
            s += d[i];
            if (mx < s) {
                mx = s;
                j = i;
            }
        }
        return j + offset;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumPopulation(vector<vector<int>>& logs) {
        int d[101]{};
        const int offset = 1950;
        for (auto& log : logs) {
            int a = log[0] - offset;
            int b = log[1] - offset;
            ++d[a];
            --d[b];
        }
        int s = 0, mx = 0;
        int j = 0;
        for (int i = 0; i < 101; ++i) {
            s += d[i];
            if (mx < s) {
                mx = s;
                j = i;
            }
        }
        return j + offset;
    }
};
```

#### Go

```go
func maximumPopulation(logs [][]int) int {
	d := [101]int{}
	offset := 1950
	for _, log := range logs {
		a, b := log[0]-offset, log[1]-offset
		d[a]++
		d[b]--
	}
	var s, mx, j int
	for i, x := range d {
		s += x
		if mx < s {
			mx = s
			j = i
		}
	}
	return j + offset
}
```

#### TypeScript

```ts
function maximumPopulation(logs: number[][]): number {
    const d: number[] = new Array(101).fill(0);
    const offset = 1950;
    for (const [birth, death] of logs) {
        d[birth - offset]++;
        d[death - offset]--;
    }
    let j = 0;
    for (let i = 0, s = 0, mx = 0; i < d.length; ++i) {
        s += d[i];
        if (mx < s) {
            mx = s;
            j = i;
        }
    }
    return j + offset;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} logs
 * @return {number}
 */
var maximumPopulation = function (logs) {
    const d = new Array(101).fill(0);
    const offset = 1950;
    for (let [a, b] of logs) {
        a -= offset;
        b -= offset;
        d[a]++;
        d[b]--;
    }
    let j = 0;
    for (let i = 0, s = 0, mx = 0; i < 101; ++i) {
        s += d[i];
        if (mx < s) {
            mx = s;
            j = i;
        }
    }
    return j + offset;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
