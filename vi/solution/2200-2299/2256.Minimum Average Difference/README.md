---
comments: true
difficulty: Medium
rating: 1394
source: Biweekly Contest 77 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2256. Minimum Average Difference](https://leetcode.com/problems/minimum-average-difference)

[中文文档](/solution/2200-2299/2256.Minimum%20Average%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>.</p>

<p><strong>Độ chênh lệch trung bình</strong> tại chỉ số <code>i</code> là <strong>giá trị tuyệt đối</strong> của <strong>hiệu</strong> giữa giá trị trung bình của <code>i + 1</code> phần tử <strong>đầu tiên</strong> trong <code>nums</code> và giá trị trung bình của <code>n - i - 1</code> phần tử <strong>cuối cùng</strong>. Cả hai giá trị trung bình đều được <strong>làm tròn xuống</strong> đến số nguyên gần nhất.</p>

<p>Hãy trả về <em>chỉ số có <strong>độ chênh lệch trung bình nhỏ nhất</strong></em>. Nếu có nhiều chỉ số như vậy, hãy trả về chỉ số <strong>nhỏ nhất</strong>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Giá trị tuyệt đối</strong> của hai số là giá trị tuyệt đối của hiệu giữa chúng.</li>
<li><strong>Giá trị trung bình</strong> của <code>n</code> phần tử là <strong>tổng</strong> của <code>n</code> phần tử chia cho <code>n</code> (<strong>phép chia nguyên</strong>).</li>
	<li>Giá trị trung bình của <code>0</code> phần tử được xem là <code>0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5,3,9,5,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Độ chênh lệch trung bình tại chỉ số 0 là: |2 / 1 - (5 + 3 + 9 + 5 + 3) / 5| = |2 / 1 - 25 / 5| = |2 - 5| = 3.
- Độ chênh lệch trung bình tại chỉ số 1 là: |(2 + 5) / 2 - (3 + 9 + 5 + 3) / 4| = |7 / 2 - 20 / 4| = |3 - 5| = 2.
- Độ chênh lệch trung bình tại chỉ số 2 là: |(2 + 5 + 3) / 3 - (9 + 5 + 3) / 3| = |10 / 3 - 17 / 3| = |3 - 5| = 2.
- Độ chênh lệch trung bình tại chỉ số 3 là: |(2 + 5 + 3 + 9) / 4 - (5 + 3) / 2| = |19 / 4 - 8 / 2| = |4 - 4| = 0.
- Độ chênh lệch trung bình tại chỉ số 4 là: |(2 + 5 + 3 + 9 + 5) / 5 - 3 / 1| = |24 / 5 - 3 / 1| = |4 - 3| = 1.
- Độ chênh lệch trung bình tại chỉ số 5 là: |(2 + 5 + 3 + 9 + 5 + 3) / 6 - 0| = |27 / 6 - 0| = |4 - 0| = 4.
Độ chênh lệch trung bình tại chỉ số 3 là nhỏ nhất, nên trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Chỉ có chỉ số 0, nên trả về 0.
Độ chênh lệch trung bình tại chỉ số 0 là: |0 / 1 - 0| = |0 - 0| = 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi cách chia, ta so sánh các giá trị trung bình nguyên của hai phía và chọn vị trí nhỏ nhất đạt giá trị nhỏ nhất. Vì $n \le 10^5$, ta không thể tính lại tổng từ đầu. Có thể duy trì tổng của hai phía trong khi duyệt.
>
> Khởi tạo với tổng bên phải $suf$, chuyển từng $x$ vào $pre$, và coi phía bên phải rỗng có giá trị trung bình bằng $0$. Ghi nhận chỉ số có độ chênh lệch tuyệt đối nhỏ nhất.

<!-- thinking:end -->

Ta duyệt trực tiếp mảng $nums$. Với mỗi chỉ số $i$, ta duy trì tổng của $i+1$ phần tử đầu tiên là $pre$ và tổng của $n-i-1$ phần tử cuối cùng là $suf$. Ta tính giá trị tuyệt đối của hiệu giữa giá trị trung bình của $i+1$ phần tử đầu tiên và giá trị trung bình của $n-i-1$ phần tử cuối cùng, gọi là $t$. Nếu $t$ nhỏ hơn giá trị nhỏ nhất hiện tại $mi$, ta cập nhật đáp án $ans=i$ và giá trị nhỏ nhất $mi=t$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumAverageDifference(self, nums: List[int]) -> int:
        pre, suf = 0, sum(nums)
        n = len(nums)
        ans, mi = 0, inf
        for i, x in enumerate(nums):
            pre += x
            suf -= x
            a = pre // (i + 1)
            b = 0 if n - i - 1 == 0 else suf // (n - i - 1)
            if (t := abs(a - b)) < mi:
                ans = i
                mi = t
        return ans
```

#### Java

```java
class Solution {
    public int minimumAverageDifference(int[] nums) {
        int n = nums.length;
        long pre = 0, suf = 0;
        for (int x : nums) {
            suf += x;
        }
        int ans = 0;
        long mi = Long.MAX_VALUE;
        for (int i = 0; i < n; ++i) {
            pre += nums[i];
            suf -= nums[i];
            long a = pre / (i + 1);
            long b = n - i - 1 == 0 ? 0 : suf / (n - i - 1);
            long t = Math.abs(a - b);
            if (t < mi) {
                ans = i;
                mi = t;
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
    int minimumAverageDifference(vector<int>& nums) {
        int n = nums.size();
        using ll = long long;
        ll pre = 0;
        ll suf = accumulate(nums.begin(), nums.end(), 0LL);
        int ans = 0;
        ll mi = suf;
        for (int i = 0; i < n; ++i) {
            pre += nums[i];
            suf -= nums[i];
            ll a = pre / (i + 1);
            ll b = n - i - 1 == 0 ? 0 : suf / (n - i - 1);
            ll t = abs(a - b);
            if (t < mi) {
                ans = i;
                mi = t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumAverageDifference(nums []int) (ans int) {
	n := len(nums)
	pre, suf := 0, 0
	for _, x := range nums {
		suf += x
	}
	mi := suf
	for i, x := range nums {
		pre += x
		suf -= x
		a := pre / (i + 1)
		b := 0
		if n-i-1 != 0 {
			b = suf / (n - i - 1)
		}
		if t := abs(a - b); t < mi {
			ans = i
			mi = t
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minimumAverageDifference(nums: number[]): number {
    const n = nums.length;
    let pre = 0;
    let suf = nums.reduce((a, b) => a + b);
    let ans = 0;
    let mi = suf;
    for (let i = 0; i < n; ++i) {
        pre += nums[i];
        suf -= nums[i];
        const a = Math.floor(pre / (i + 1));
        const b = n - i - 1 === 0 ? 0 : Math.floor(suf / (n - i - 1));
        const t = Math.abs(a - b);
        if (t < mi) {
            ans = i;
            mi = t;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
