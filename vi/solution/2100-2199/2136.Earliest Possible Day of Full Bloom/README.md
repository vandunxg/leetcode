---
comments: true
difficulty: Hard
rating: 2033
source: Weekly Contest 275 Q4
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2136. Earliest Possible Day of Full Bloom](https://leetcode.com/problems/earliest-possible-day-of-full-bloom)

[中文文档](/solution/2100-2199/2136.Earliest%20Possible%20Day%20of%20Full%20Bloom/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> hạt giống hoa. Mỗi hạt giống phải được gieo trồng xong trước khi bắt đầu phát triển, rồi nở hoa. Việc gieo trồng một hạt giống mất một khoảng thời gian, và hạt giống cũng cần thời gian để phát triển. Cho hai mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>plantTime</code> và <code>growTime</code>, mỗi mảng có độ dài <code>n</code>:</p>

<ul>
	<li><code>plantTime[i]</code> là số <strong>ngày trọn vẹn</strong> cần để bạn <strong>gieo trồng</strong> hạt giống thứ <code>i<sup>th</sup></code>. Mỗi ngày, bạn có thể gieo trồng đúng một hạt giống. Bạn <strong>không</strong> cần gieo cùng một hạt giống trong các ngày liên tiếp, nhưng việc gieo một hạt giống chỉ hoàn tất <strong>sau khi</strong> bạn đã dành tổng cộng <code>plantTime[i]</code> ngày để gieo hạt đó.</li>
	<li><code>growTime[i]</code> là số <strong>ngày trọn vẹn</strong> cần để hạt giống thứ <code>i<sup>th</sup></code> phát triển sau khi được gieo trồng hoàn toàn. <strong>Sau</strong> ngày phát triển cuối cùng, hoa <strong>nở</strong> và duy trì trạng thái nở mãi mãi.</li>
</ul>

<p>Từ đầu ngày <code>0</code>, bạn có thể gieo trồng các hạt giống theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>Hãy trả về <em>ngày <strong>sớm nhất</strong> mà <strong>tất cả</strong> hạt giống đều đang nở hoa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2136.Earliest%20Possible%20Day%20of%20Full%20Bloom/images/1.png" style="width: 453px; height: 149px;" />
<pre>
<strong>Đầu vào:</strong> plantTime = [1,4,3], growTime = [2,3,1]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Những chậu màu xám biểu thị các ngày gieo trồng, những chậu có màu biểu thị các ngày phát triển, và bông hoa biểu thị ngày hoa nở.
Một cách tối ưu là:
Vào ngày 0, gieo hạt giống thứ 0<sup>th</sup>. Hạt giống phát triển trong 2 ngày trọn vẹn và nở vào ngày 3.
Vào các ngày 1, 2, 3 và 4, gieo hạt giống thứ 1<sup>st</sup>. Hạt giống phát triển trong 3 ngày trọn vẹn và nở vào ngày 8.
Vào các ngày 5, 6 và 7, gieo hạt giống thứ 2<sup>nd</sup>. Hạt giống phát triển trong 1 ngày trọn vẹn và nở vào ngày 9.
Vì vậy, vào ngày 9, tất cả hạt giống đều đang nở hoa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2136.Earliest%20Possible%20Day%20of%20Full%20Bloom/images/2.png" style="width: 454px; height: 184px;" />
<pre>
<strong>Đầu vào:</strong> plantTime = [1,2,3,2], growTime = [2,1,2,1]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Những chậu màu xám biểu thị các ngày gieo trồng, những chậu có màu biểu thị các ngày phát triển, và bông hoa biểu thị ngày hoa nở.
Một cách tối ưu là:
Vào ngày 1, gieo hạt giống thứ 0<sup>th</sup>. Hạt giống phát triển trong 2 ngày trọn vẹn và nở vào ngày 4.
Vào các ngày 0 và 3, gieo hạt giống thứ 1<sup>st</sup>. Hạt giống phát triển trong 1 ngày trọn vẹn và nở vào ngày 5.
Vào các ngày 2, 4 và 5, gieo hạt giống thứ 2<sup>nd</sup>. Hạt giống phát triển trong 2 ngày trọn vẹn và nở vào ngày 8.
Vào các ngày 6 và 7, gieo hạt giống thứ 3<sup>rd</sup>. Hạt giống phát triển trong 1 ngày trọn vẹn và nở vào ngày 9.
Vì vậy, vào ngày 9, tất cả hạt giống đều đang nở hoa.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> plantTime = [1], growTime = [1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Vào ngày 0, gieo hạt giống thứ 0<sup>th</sup>. Hạt giống phát triển trong 1 ngày trọn vẹn và nở vào ngày 2.
Vì vậy, vào ngày 2, tất cả hạt giống đều đang nở hoa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == plantTime.length == growTime.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= plantTime[i], growTime[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi ngày chỉ có thể gieo một hạt giống, nên tổng thời gian gieo không phụ thuộc vào thứ tự. Ngày hoa nở là giá trị lớn nhất của “thời điểm gieo xong cộng với thời gian phát triển”. Nếu gieo những hạt có thời gian phát triển ngắn trước, các hạt có thời gian phát triển dài sẽ bị trì hoãn.
>
> Những hạt có $\textit{growTime}$ lớn hơn nên được bắt đầu sớm hơn, vì vậy ta gieo theo thứ tự giảm dần của thời gian phát triển. Biến tiền tố $t$ cộng dồn $\textit{plantTime}$, và hạt đó nở vào thời điểm $t+\textit{growTime}$; đáp án là giá trị lớn nhất.
>
> Chỉ cần duyệt một lần theo thứ tự đã sắp xếp là tìm được ngày tất cả hoa nở sớm nhất.

<!-- thinking:end -->

Theo mô tả đề bài, ta biết rằng mỗi ngày chỉ có thể gieo một hạt giống. Do đó, bất kể thứ tự gieo như thế nào, tổng thời gian gieo của tất cả hạt giống luôn bằng $\sum_{i=0}^{n-1} plantTime[i]$. Để tất cả hạt giống nở sớm nhất có thể, ta nên ưu tiên gieo những hạt có thời gian phát triển dài nhất. Vì vậy, ta có thể sắp xếp tất cả hạt giống theo thời gian phát triển giảm dần, rồi gieo chúng lần lượt.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng hạt giống.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def earliestFullBloom(self, plantTime: List[int], growTime: List[int]) -> int:
        ans = t = 0
        for pt, gt in sorted(zip(plantTime, growTime), key=lambda x: -x[1]):
            t += pt
            ans = max(ans, t + gt)
        return ans
```

#### Java

```java
class Solution {
    public int earliestFullBloom(int[] plantTime, int[] growTime) {
        int n = plantTime.length;
        Integer[] idx = new Integer[n];
        for (int i = 0; i < n; i++) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> growTime[j] - growTime[i]);
        int ans = 0, t = 0;
        for (int i : idx) {
            t += plantTime[i];
            ans = Math.max(ans, t + growTime[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int earliestFullBloom(vector<int>& plantTime, vector<int>& growTime) {
        int n = plantTime.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return growTime[j] < growTime[i]; });
        int ans = 0, t = 0;
        for (int i : idx) {
            t += plantTime[i];
            ans = max(ans, t + growTime[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func earliestFullBloom(plantTime []int, growTime []int) (ans int) {
	n := len(plantTime)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool { return growTime[idx[j]] < growTime[idx[i]] })
	t := 0
	for _, i := range idx {
		t += plantTime[i]
		ans = max(ans, t+growTime[i])
	}
	return
}
```

#### TypeScript

```ts
function earliestFullBloom(plantTime: number[], growTime: number[]): number {
    const n = plantTime.length;
    const idx: number[] = Array.from({ length: n }, (_, i) => i);
    idx.sort((i, j) => growTime[j] - growTime[i]);
    let [ans, t] = [0, 0];
    for (const i of idx) {
        t += plantTime[i];
        ans = Math.max(ans, t + growTime[i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn earliest_full_bloom(plant_time: Vec<i32>, grow_time: Vec<i32>) -> i32 {
        let mut idx: Vec<usize> = (0..plant_time.len()).collect();
        idx.sort_by_key(|&i| -&grow_time[i]);
        let mut ans = 0;
        let mut t = 0;
        for &i in &idx {
            t += plant_time[i];
            ans = ans.max(t + grow_time[i]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
