---
comments: true
difficulty: Hard
rating: 2277
source: Weekly Contest 461 Q4
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3640. Trionic Array II](https://leetcode.com/problems/trionic-array-ii)

[中文文档](/solution/3600-3699/3640.Trionic%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p data-end="191" data-start="0">Bạn được cho một mảng số nguyên <code data-end="61" data-start="55">nums</code> có độ dài <code data-end="75" data-start="72">n</code>.</p>

<p data-end="191" data-start="0"><strong data-end="99" data-is-only-node="" data-start="79">Mảng con trionic</strong> là một mảng con liên tiếp <code data-end="136" data-start="125">nums[l...r]</code> (với <code data-end="158" data-start="143">0 &lt;= l &lt; r &lt; n</code>) sao cho tồn tại các chỉ số <code>l &lt; p &lt; q &lt; r</code> thỏa mãn:</p>

<ul>
	<li data-end="267" data-start="230"><code data-end="241" data-start="230">nums[l...p]</code> tăng <strong>chặt</strong>,</li>
	<li data-end="307" data-start="270"><code data-end="281" data-start="270">nums[p...q]</code> giảm <strong>chặt</strong>,</li>
	<li data-end="347" data-start="310"><code data-end="321" data-start="310">nums[q...r]</code> tăng <strong>chặt</strong>.</li>
</ul>

<p data-end="609" data-is-last-node="" data-is-only-node="" data-start="349">Trả về <strong>tổng lớn nhất</strong> của một mảng con trionic bất kỳ trong <code data-end="417" data-start="411">nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,-2,-1,-3,0,2,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-4</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="129" data-start="72">Chọn <code data-end="99" data-start="92">l = 1</code>, <code data-end="108" data-start="101">p = 2</code>, <code data-end="117" data-start="110">q = 3</code>, <code data-end="126" data-start="119">r = 5</code>:</p>

<ul>
	<li data-end="203" data-start="132"><code data-end="166" data-start="132">nums[l...p] = nums[1...2] = [-2, -1]</code> tăng chặt (<code data-end="200" data-start="191">-2 &lt; -1</code>).</li>
	<li data-end="277" data-start="206"><code data-end="240" data-start="206">nums[p...q] = nums[2...3] = [-1, -3]</code> giảm chặt (<code data-end="274" data-start="265">-1 &gt; -3</code>)</li>
	<li data-end="396" data-start="280"><code data-end="316" data-start="280">nums[q...r] = nums[3...5] = [-3, 0, 2]</code> tăng chặt (<code data-end="353" data-start="341">-3 &lt; 0 &lt; 2</code>).</li>
	<li data-end="396" data-start="280">Tổng = <code>(-2) + (-1) + (-3) + 0 + 2 = -4</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="519" data-start="462">Chọn <code data-end="489" data-start="482">l = 0</code>, <code data-end="498" data-start="491">p = 1</code>, <code data-end="507" data-start="500">q = 2</code>, <code data-end="516" data-start="509">r = 3</code>:</p>

<ul>
	<li data-end="589" data-start="522"><code data-end="554" data-start="522">nums[l...p] = nums[0...1] = [1, 4]</code> tăng chặt (<code data-end="586" data-start="579">1 &lt; 4</code>).</li>
	<li data-end="659" data-start="592"><code data-end="624" data-start="592">nums[p...q] = nums[1...2] = [4, 2]</code> giảm chặt (<code data-end="656" data-start="649">4 &gt; 2</code>).</li>
	<li data-end="754" data-is-last-node="" data-start="662"><code data-end="694" data-start="662">nums[q...r] = nums[2...3] = [2, 7]</code> tăng chặt (<code data-end="726" data-start="719">2 &lt; 7</code>).</li>
	<li data-end="754" data-is-last-node="" data-start="662">Tổng = <code>1 + 4 + 2 + 7 = 14</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li data-end="883" data-start="851"><code data-end="881" data-start="851">4 &lt;= n = nums.length &lt;= 10<sup>5</sup></code></li>
	<li data-end="914" data-start="886"><code data-end="912" data-start="886">-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li data-end="978" data-is-last-node="" data-start="917">Đảm bảo tồn tại ít nhất một mảng con trionic.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Vòng lặp theo nhóm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần mảng con trionic có tổng lớn nhất. Việc liệt kê các đỉnh sẽ có độ phức tạp bậc hai. Các đoạn trionic liền kề dùng chung một đoạn tăng, vì vậy ta có thể duyệt theo nhóm để liệt kê chúng trong thời gian tuyến tính.
>
> Một con trỏ lần lượt cắt một đoạn tăng, một đoạn giảm và một đoạn tăng; nếu đoạn giữa bị suy biến hoặc đã đến cuối mảng thì bỏ qua. Tổng của một đoạn cực đại gồm phần giữa cố định, cộng với hậu tố tốt nhất mở rộng sang trái của đoạn tăng đầu tiên và tiền tố tốt nhất mở rộng sang phải của đoạn tăng cuối cùng.
>
> Đoạn tăng thứ ba có thể bắt đầu đoạn tiếp theo, nên con trỏ quay lại đỉnh thung lũng $q$. Mỗi chỉ số được truy cập một số lần không đổi.

<!-- thinking:end -->

Ta có thể duyệt qua mảng để tìm tất cả các mảng con trionic cực đại có thể có, tính tổng của chúng và cập nhật giá trị lớn nhất.

Ta định nghĩa một con trỏ $i$, ban đầu $i = 0$, biểu diễn vị trí hiện tại và trỏ đến phần tử đầu tiên của mảng. Ta di chuyển $i$ sang phải cho đến khi tìm thấy phần tử đầu tiên không thỏa mãn điều kiện tăng chặt, tức là $nums[i-1] \geq nums[i]$. Nếu lúc này $i = l + 1$, nghĩa là đoạn này chỉ có một phần tử và không thể tạo thành một dãy tăng, nên ta tiếp tục vòng lặp tiếp theo.

Tiếp theo, ta định nghĩa con trỏ $p$, biểu diễn vị trí cuối của đoạn tăng hiện tại. Sau đó, ta tìm đoạn giảm chặt thứ hai. Nếu đoạn này chỉ có một phần tử, đã đến cuối mảng hoặc gặp các phần tử bằng nhau, ta tiếp tục vòng lặp tiếp theo.

Tiếp đó, ta định nghĩa con trỏ $q$, biểu diễn vị trí cuối của đoạn giảm hiện tại. Sau đó, ta tìm đoạn tăng chặt thứ ba. Lúc này, ta đã tìm được một mảng con trionic cực đại. Tổng lớn nhất của mảng con trionic này gồm các phần sau:

- Tổng các phần tử trong đoạn chỉ số $[p-2,..,q+1]$
- Tổng của mảng con tăng lớn nhất mở rộng sang trái từ $p-3$, hoặc 0 nếu không tồn tại
- Tổng của mảng con tăng lớn nhất mở rộng sang phải từ $q+2$, hoặc 0 nếu không tồn tại.

Sau khi tính tổng của mảng con trionic này, ta cập nhật đáp án. Sau đó, ta di chuyển con trỏ $i$ đến vị trí $q$, vì phần tăng của đoạn thứ ba có thể được dùng làm đoạn tăng đầu tiên trong lần lặp tiếp theo.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$, chỉ sử dụng lượng không gian phụ hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumTrionic(self, nums: List[int]) -> int:
        n = len(nums)
        i = 0
        ans = -inf
        while i < n:
            l = i
            i += 1
            while i < n and nums[i - 1] < nums[i]:
                i += 1
            if i == l + 1:
                continue

            p = i - 1
            s = nums[p - 1] + nums[p]
            while i < n and nums[i - 1] > nums[i]:
                s += nums[i]
                i += 1
            if i == p + 1 or i == n or nums[i - 1] == nums[i]:
                continue

            q = i - 1
            s += nums[i]
            i += 1
            mx = t = 0
            while i < n and nums[i - 1] < nums[i]:
                t += nums[i]
                i += 1
                mx = max(mx, t)
            s += mx

            mx = t = 0
            for j in range(p - 2, l - 1, -1):
                t += nums[j]
                mx = max(mx, t)
            s += mx

            ans = max(ans, s)
            i = q
        return ans
```

#### Java

```java
class Solution {
    public long maxSumTrionic(int[] nums) {
        int n = nums.length;
        int i = 0;
        long ans = Long.MIN_VALUE;
        while (i < n) {
            int l = i;
            i += 1;
            while (i < n && nums[i - 1] < nums[i]) {
                i += 1;
            }
            if (i == l + 1) {
                continue;
            }

            int p = i - 1;
            long s = nums[p - 1] + nums[p];
            while (i < n && nums[i - 1] > nums[i]) {
                s += nums[i];
                i += 1;
            }
            if (i == p + 1 || i == n || nums[i - 1] == nums[i]) {
                continue;
            }

            int q = i - 1;
            s += nums[i];
            i += 1;
            long mx = 0, t = 0;
            while (i < n && nums[i - 1] < nums[i]) {
                t += nums[i];
                i += 1;
                mx = Math.max(mx, t);
            }
            s += mx;

            mx = 0;
            t = 0;
            for (int j = p - 2; j >= l; j--) {
                t += nums[j];
                mx = Math.max(mx, t);
            }
            s += mx;

            ans = Math.max(ans, s);
            i = q;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxSumTrionic(vector<int>& nums) {
        int n = nums.size();
        int i = 0;
        long long ans = LLONG_MIN;
        while (i < n) {
            int l = i;
            i += 1;
            while (i < n && nums[i - 1] < nums[i]) {
                i += 1;
            }
            if (i == l + 1) {
                continue;
            }

            int p = i - 1;
            long long s = nums[p - 1] + nums[p];
            while (i < n && nums[i - 1] > nums[i]) {
                s += nums[i];
                i += 1;
            }
            if (i == p + 1 || i == n || nums[i - 1] == nums[i]) {
                continue;
            }

            int q = i - 1;
            s += nums[i];
            i += 1;
            long long mx = 0, t = 0;
            while (i < n && nums[i - 1] < nums[i]) {
                t += nums[i];
                i += 1;
                mx = max(mx, t);
            }
            s += mx;

            mx = 0, t = 0;
            for (int j = p - 2; j >= l; j--) {
                t += nums[j];
                mx = max(mx, t);
            }
            s += mx;

            ans = max(ans, s);
            i = q;
        }
        return ans;
    }
};
```

#### Go

```go
func maxSumTrionic(nums []int) int64 {
	n := len(nums)
	i := 0
	ans := int64(math.MinInt64)
	for i < n {
		l := i
		for i++; i < n && nums[i-1] < nums[i]; {
			i++
		}
		if i == l+1 {
			continue
		}

		p := i - 1
		s := int64(nums[p-1]) + int64(nums[p])
		for i < n && nums[i-1] > nums[i] {
			s += int64(nums[i])
			i++
		}
		if i == p+1 || i == n || nums[i-1] == nums[i] {
			continue
		}

		q := i - 1
		s += int64(nums[i])
		i++
		var mx, t int64
		for i < n && nums[i-1] < nums[i] {
			t += int64(nums[i])
			i++
			mx = max(mx, t)
		}
		s += mx

		mx, t = 0, 0
		for j := p - 2; j >= l; j-- {
			t += int64(nums[j])
			mx = max(mx, t)
		}
		s += mx

		ans = max(ans, s)
		i = q
	}
	return ans
}
```

#### TypeScript

```ts
function maxSumTrionic(nums: number[]): number {
    const n = nums.length;
    let i = 0;
    let ans = -Infinity;

    while (i < n) {
        const l = i;
        i += 1;

        while (i < n && nums[i - 1] < nums[i]) i += 1;
        if (i === l + 1) continue;

        const p = i - 1;
        let s = nums[p - 1] + nums[p];

        while (i < n && nums[i - 1] > nums[i]) {
            s += nums[i];
            i += 1;
        }
        if (i === p + 1 || i === n || nums[i - 1] === nums[i]) continue;

        const q = i - 1;
        s += nums[i];
        i += 1;

        let mx = 0,
            t = 0;
        while (i < n && nums[i - 1] < nums[i]) {
            t += nums[i];
            i += 1;
            mx = Math.max(mx, t);
        }
        s += mx;

        mx = 0;
        t = 0;
        for (let j = p - 2; j >= l; j--) {
            t += nums[j];
            mx = Math.max(mx, t);
        }
        s += mx;

        ans = Math.max(ans, s);
        i = q;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_sum_trionic(nums: Vec<i32>) -> i64 {
        let n = nums.len();
        let mut i = 0;
        let mut ans = i64::MIN;
        while i < n {
            let l = i;
            i += 1;
            while i < n && nums[i - 1] < nums[i] {
                i += 1;
            }
            if i == l + 1 {
                continue;
            }

            let p = i - 1;
            let mut s = nums[p - 1] as i64 + nums[p] as i64;
            while i < n && nums[i - 1] > nums[i] {
                s += nums[i] as i64;
                i += 1;
            }
            if i == p + 1 || i == n || nums[i - 1] == nums[i] {
                continue;
            }

            let q = i - 1;
            s += nums[i] as i64;
            i += 1;
            let mut mx = 0i64;
            let mut t = 0i64;
            while i < n && nums[i - 1] < nums[i] {
                t += nums[i] as i64;
                i += 1;
                mx = mx.max(t);
            }
            s += mx;

            mx = 0;
            t = 0;
            let mut j = p as isize - 2;
            while j >= l as isize {
                t += nums[j as usize] as i64;
                mx = mx.max(t);
                j -= 1;
            }
            s += mx;

            ans = ans.max(s);
            i = q;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
