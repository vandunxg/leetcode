---
comments: true
difficulty: Hard
rating: 1679
source: Biweekly Contest 23 Q4
tags:
    - Greedy
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1402. Reducing Dishes](https://leetcode.com/problems/reducing-dishes)

[中文文档](/solution/1400-1499/1402.Reducing%20Dishes/README.md)

## Mô tả

<!-- description:start -->

<p>Một đầu bếp đã thu thập dữ liệu về mức độ <code>satisfaction</code> của <code>n</code> món ăn. Đầu bếp có thể nấu mỗi món trong 1 đơn vị thời gian.</p>

<p><strong>Hệ số thời gian yêu thích</strong> của một món ăn được định nghĩa là thời gian nấu món đó, bao gồm cả các món trước đó, nhân với mức độ hài lòng của món, tức là <code>time[i] * satisfaction[i]</code>.</p>

<p>Hãy trả về tổng <strong>hệ số thời gian yêu thích </strong> lớn nhất mà đầu bếp có thể đạt được sau khi chuẩn bị một số món ăn.</p>

<p>Các món ăn có thể được chuẩn bị theo <strong>bất kỳ </strong>thứ tự nào và đầu bếp có thể bỏ đi một số món để đạt được giá trị lớn nhất này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> satisfaction = [-1,-8,0,5,-9]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Sau khi bỏ món thứ hai và món cuối cùng, tổng <strong>hệ số thời gian yêu thích</strong> lớn nhất sẽ bằng (-1*1 + 0*2 + 5*3 = 14).
Mỗi món ăn được chuẩn bị trong một đơn vị thời gian.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> satisfaction = [4,3,2]
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Các món ăn có thể được chuẩn bị theo bất kỳ thứ tự nào, (2*1 + 3*2 + 4*3 = 20)
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> satisfaction = [-1,-4,-5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi người không thích các món ăn này. Không có món nào được chuẩn bị.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == satisfaction.length</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>-1000 &lt;= satisfaction[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Việc thử mọi tập con và mọi thứ tự nấu là bất khả thi với $n\le 500$. Mức độ hài lòng có thể âm và thực đơn rỗng có điểm số $0$, vì vậy ta chỉ cần tìm tổng hệ số thời gian yêu thích lớn nhất.
>
> Trong một tập đã chọn, các giá trị lớn hơn nên nhận hệ số thời gian lớn hơn, nên thứ tự tối ưu là tăng dần. Việc thêm món có giá trị lớn tiếp theo làm tổng tăng thêm bằng tổng các món đã chọn.
>
> Sắp xếp theo thứ tự giảm dần, duy trì tổng tiền tố $s$, và cộng $s$ vào đáp án khi $s>0$. Khi tổng tiền tố không dương, các món nhỏ hơn không thể giúp cải thiện kết quả.

<!-- thinking:end -->

Giả sử ta chỉ chọn một món, khi đó ta nên chọn món có mức độ hài lòng cao nhất $s_0$, rồi kiểm tra xem $s_0$ có lớn hơn 0 hay không. Nếu $s_0 \leq 0$, ta không nấu món nào; ngược lại, ta nấu món này và tổng mức độ hài lòng là $s_0$.

Nếu chọn hai món, ta nên chọn hai món có mức độ hài lòng cao nhất là $s_0$ và $s_1$, khi đó mức độ hài lòng là $s_1 + 2 \times s_0$. Lúc này, ta cần đảm bảo mức độ hài lòng sau khi chọn lớn hơn trước khi chọn, tức là $s_1 + 2 \times s_0 > s_0$, tương đương với việc chỉ cần $s_1 + s_0 > 0$ thì ta có thể chọn hai món này.

Tương tự, ta có thể rút ra quy tắc: nên chọn $k$ món có mức độ hài lòng cao nhất và đảm bảo tổng mức độ hài lòng của $k$ món đầu tiên lớn hơn $0$.

Khi triển khai, trước tiên ta có thể sắp xếp mức độ hài lòng của tất cả món ăn, sau đó bắt đầu chọn từ món có mức độ hài lòng cao nhất. Mỗi lần thêm mức độ hài lòng của món hiện tại, nếu tổng nhỏ hơn hoặc bằng $0$ thì không chọn các món phía sau nữa; ngược lại, ta chọn món này.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSatisfaction(self, satisfaction: List[int]) -> int:
        satisfaction.sort(reverse=True)
        ans = s = 0
        for x in satisfaction:
            s += x
            if s <= 0:
                break
            ans += s
        return ans
```

#### Java

```java
class Solution {
    public int maxSatisfaction(int[] satisfaction) {
        Arrays.sort(satisfaction);
        int ans = 0, s = 0;
        for (int i = satisfaction.length - 1; i >= 0; --i) {
            s += satisfaction[i];
            if (s <= 0) {
                break;
            }
            ans += s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSatisfaction(vector<int>& satisfaction) {
        sort(rbegin(satisfaction), rend(satisfaction));
        int ans = 0, s = 0;
        for (int x : satisfaction) {
            s += x;
            if (s <= 0) {
                break;
            }
            ans += s;
        }
        return ans;
    }
};
```

#### Go

```go
func maxSatisfaction(satisfaction []int) (ans int) {
	sort.Slice(satisfaction, func(i, j int) bool { return satisfaction[i] > satisfaction[j] })
	s := 0
	for _, x := range satisfaction {
		s += x
		if s <= 0 {
			break
		}
		ans += s
	}
	return
}
```

#### TypeScript

```ts
function maxSatisfaction(satisfaction: number[]): number {
    satisfaction.sort((a, b) => b - a);
    let [ans, s] = [0, 0];
    for (const x of satisfaction) {
        s += x;
        if (s <= 0) {
            break;
        }
        ans += s;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
