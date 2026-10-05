---
comments: true
difficulty: Medium
rating: 1572
source: Biweekly Contest 185 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3964. Minimum Lights to Illuminate a Road](https://leetcode.com/problems/minimum-lights-to-illuminate-a-road)

[中文文档](/solution/3900-3999/3964.Minimum%20Lights%20to%20Illuminate%20a%20Road/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>lights</code> có độ dài <code>n</code>, biểu thị các vị trí từ 0 đến <code>n - 1</code> trên một con đường.</p>

<p>Với mỗi vị trí <code>i</code>:</p>

<ul>
	<li>Nếu <code>lights[i] = v</code>, trong đó <code>v &gt; 0</code>, có một bóng đèn đang hoạt động tại vị trí <code>i</code>, <strong>chiếu sáng</strong> mọi vị trí từ <code>max(0, i - v)</code> đến <code>min(n - 1, i + v)</code>, bao gồm cả hai đầu mút.</li>
	<li>Nếu <code>lights[i] = 0</code>, không có bóng đèn đang hoạt động tại vị trí <code>i</code>.</li>
</ul>

<p>Một vị trí được gọi là <strong>nhìn thấy</strong> nếu được <strong>ít nhất</strong> một bóng đèn đang hoạt động chiếu sáng.</p>

<p>Bạn có thể lắp đặt <strong>thêm</strong> bóng đèn tại <strong>bất kỳ</strong> vị trí nào. Mỗi bóng đèn thêm tại vị trí <code>j</code> <strong>chiếu sáng</strong> các vị trí từ <code>max(0, j - 1)</code> đến <code>min(n - 1, j + 1)</code>, bao gồm cả hai đầu mút.</p>

<p>Trả về số bóng đèn thêm ít nhất cần lắp để làm cho <strong>mọi</strong> vị trí trên đường đều nhìn thấy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lights = [0,0,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách lắp đặt tối ưu là:</p>

<ul>
	<li>Lắp thêm một bóng đèn tại vị trí 1, chiếu sáng các vị trí <code>[0, 1, 2]</code>.</li>
	<li>Lắp thêm một bóng đèn tại vị trí 3, chiếu sáng các vị trí <code>[2, 3]</code>.</li>
</ul>

<p>Vì vậy, số bóng đèn thêm ít nhất cần lắp là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">lights = [0,0,0,2,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì <code>lights[3] = 2</code>, bóng đèn đang hoạt động tại vị trí 3 chiếu sáng các vị trí <code>[1, 2, 3, 4]</code>.</li>
	<li>Lắp thêm một bóng đèn tại vị trí 1 sẽ chiếu sáng các vị trí <code>[0, 1, 2]</code>, khiến mọi vị trí đều nhìn thấy.</li>
	<li>Vì vậy, số bóng đèn thêm ít nhất cần lắp là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == lights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= lights[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Các bóng đèn có sẵn tạo vùng chiếu sáng; mỗi đoạn liên tiếp chưa được chiếu sáng cần bóng đèn mới, và mỗi bóng đèn chỉ chiếu sáng nhiều nhất ba ô trống liên tiếp. Vì $n\le 10^5$, mảng hiệu ghi lại đoạn $[i-v,i+v]$ của mỗi bóng đèn, còn prefix sum cho biết vùng nào đã được chiếu sáng.
>
> Sau đó duyệt các đoạn liên tiếp dài $k$ gồm các số 0 và cộng thêm $\lceil(k+2)/3\rceil$. Mảng hiệu giúp toàn bộ quá trình có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta nhận thấy với mỗi vị trí $i$, nếu $lights[i] = v$ với $v > 0$, thì vị trí $i$ được chiếu sáng và phạm vi chiếu sáng là $[i - v, i + v]$. Ta có thể sử dụng mảng hiệu để duy trì phạm vi chiếu sáng tại mỗi vị trí.

Ta định nghĩa một mảng $d$ có độ dài $n$. Với mỗi vị trí $i$, nếu $lights[i] = v$ với $v > 0$, ta cộng $1$ vào $d[i - v]$ và trừ $1$ khỏi $d[i + v + 1]$.

Sau đó, ta tính prefix sum của $d$ để thu được trạng thái chiếu sáng tại mỗi vị trí.

Cuối cùng, ta duyệt $d$, tìm độ dài của từng đoạn liên tiếp gồm các số $0$. Nếu độ dài là $k$, ta cần lắp $\lceil \frac{k + 2}{3} \rceil$ bóng đèn. Ta cộng dồn kết quả tương ứng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số vị trí trên đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLights(self, lights: list[int]) -> int:
        n = len(lights)
        d = [0] * n
        for i, v in enumerate(lights):
            if v > 0:
                l = max(0, i - v)
                r = min(n - 1, i + v)
                d[l] += 1
                if r + 1 < n:
                    d[r + 1] -= 1
        s = cnt = 0
        ans = 0
        for x in d:
            s += x
            if s == 0:
                cnt += 1
            else:
                ans += (cnt + 2) // 3
                cnt = 0
        ans += (cnt + 2) // 3
        return ans
```

#### Java

```java
class Solution {
    public int minLights(int[] lights) {
        int n = lights.length;
        int[] d = new int[n];

        for (int i = 0; i < n; i++) {
            int v = lights[i];
            if (v > 0) {
                int l = Math.max(0, i - v);
                int r = Math.min(n - 1, i + v);
                d[l]++;
                if (r + 1 < n) {
                    d[r + 1]--;
                }
            }
        }

        int s = 0, cnt = 0, ans = 0;
        for (int x : d) {
            s += x;
            if (s == 0) {
                cnt++;
            } else {
                ans += (cnt + 2) / 3;
                cnt = 0;
            }
        }

        ans += (cnt + 2) / 3;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLights(vector<int>& lights) {
        int n = lights.size();
        vector<int> d(n);

        for (int i = 0; i < n; ++i) {
            int v = lights[i];
            if (v > 0) {
                int l = max(0, i - v);
                int r = min(n - 1, i + v);
                ++d[l];
                if (r + 1 < n) {
                    --d[r + 1];
                }
            }
        }

        int s = 0, cnt = 0, ans = 0;
        for (int x : d) {
            s += x;
            if (s == 0) {
                ++cnt;
            } else {
                ans += (cnt + 2) / 3;
                cnt = 0;
            }
        }

        ans += (cnt + 2) / 3;
        return ans;
    }
};
```

#### Go

```go
func minLights(lights []int) int {
	n := len(lights)
	d := make([]int, n)

	for i, v := range lights {
		if v > 0 {
			l := max(0, i-v)
			r := min(n-1, i+v)
			d[l]++
			if r+1 < n {
				d[r+1]--
			}
		}
	}

	s, cnt, ans := 0, 0, 0
	for _, x := range d {
		s += x
		if s == 0 {
			cnt++
		} else {
			ans += (cnt + 2) / 3
			cnt = 0
		}
	}

	ans += (cnt + 2) / 3
	return ans
}
```

#### TypeScript

```ts
function minLights(lights: number[]): number {
    const n = lights.length;
    const d: number[] = Array(n).fill(0);

    for (let i = 0; i < n; i++) {
        const v = lights[i];
        if (v > 0) {
            const l = Math.max(0, i - v);
            const r = Math.min(n - 1, i + v);
            d[l]++;
            if (r + 1 < n) {
                d[r + 1]--;
            }
        }
    }

    let s = 0,
        cnt = 0,
        ans = 0;
    for (const x of d) {
        s += x;
        if (s === 0) {
            cnt++;
        } else {
            ans += Math.floor((cnt + 2) / 3);
            cnt = 0;
        }
    }

    ans += Math.floor((cnt + 2) / 3);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
