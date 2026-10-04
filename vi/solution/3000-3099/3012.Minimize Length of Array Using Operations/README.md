---
comments: true
difficulty: Medium
rating: 1832
source: Biweekly Contest 122 Q3
tags:
    - Greedy
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3012. Minimize Length of Array Using Operations](https://leetcode.com/problems/minimize-length-of-array-using-operations)

[中文文档](/solution/3000-3099/3012.Minimize%20Length%20of%20Array%20Using%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, chứa các số nguyên <strong>dương</strong>.</p>

<p>Nhiệm vụ của bạn là <strong>tối thiểu hóa</strong> độ dài của <code>nums</code> bằng cách thực hiện các thao tác sau một số lần <strong>bất kỳ</strong> (bao gồm cả không lần nào):</p>

<ul>
	<li>Chọn <strong>hai</strong> chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> trong <code>nums</code>, sao cho <code>nums[i] &gt; 0</code> và <code>nums[j] &gt; 0</code>.</li>
	<li>Chèn kết quả của <code>nums[i] % nums[j]</code> vào cuối <code>nums</code>.</li>
	<li>Xóa các phần tử tại chỉ số <code>i</code> và <code>j</code> khỏi <code>nums</code>.</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị <strong>độ dài</strong> <strong>nhỏ nhất</strong> của </em><code>nums</code><em> sau khi thực hiện các thao tác trên một số lần bất kỳ.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,3,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Một cách để tối thiểu hóa độ dài của mảng là thực hiện như sau:
Thao tác 1: Chọn các chỉ số 2 và 1, chèn nums[2] % nums[1] vào cuối, khi đó mảng trở thành [1,4,3,1,3], sau đó xóa các phần tử tại chỉ số 2 và 1.
nums trở thành [1,1,3].
Thao tác 2: Chọn các chỉ số 1 và 2, chèn nums[1] % nums[2] vào cuối, khi đó mảng trở thành [1,1,3,1], sau đó xóa các phần tử tại chỉ số 1 và 2.
nums trở thành [1,1].
Thao tác 3: Chọn các chỉ số 1 và 0, chèn nums[1] % nums[0] vào cuối, khi đó mảng trở thành [1,1,0], sau đó xóa các phần tử tại chỉ số 1 và 0.
nums trở thành [0].
Không thể giảm thêm độ dài của nums. Do đó, đáp án là 1.
Có thể chứng minh rằng 1 là độ dài nhỏ nhất có thể đạt được. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,5,10,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một cách để tối thiểu hóa độ dài của mảng là thực hiện như sau:
Thao tác 1: Chọn các chỉ số 0 và 3, chèn nums[0] % nums[3] vào cuối, khi đó mảng trở thành [5,5,5,10,5,5], sau đó xóa các phần tử tại chỉ số 0 và 3.
nums trở thành [5,5,5,5].
Thao tác 2: Chọn các chỉ số 2 và 3, chèn nums[2] % nums[3] vào cuối, khi đó mảng trở thành [5,5,5,5,0], sau đó xóa các phần tử tại chỉ số 2 và 3.
nums trở thành [5,5,0].
Thao tác 3: Chọn các chỉ số 0 và 1, chèn nums[0] % nums[1] vào cuối, khi đó mảng trở thành [5,5,0,0], sau đó xóa các phần tử tại chỉ số 0 và 1.
nums trở thành [0,0].
Không thể giảm thêm độ dài của nums. Do đó, đáp án là 2.
Có thể chứng minh rằng 2 là độ dài nhỏ nhất có thể đạt được. </pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Một cách để tối thiểu hóa độ dài của mảng là thực hiện như sau:
Thao tác 1: Chọn các chỉ số 1 và 2, chèn nums[1] % nums[2] vào cuối, khi đó mảng trở thành [2,3,4,3], sau đó xóa các phần tử tại chỉ số 1 và 2.
nums trở thành [2,3].
Thao tác 2: Chọn các chỉ số 1 và 0, chèn nums[1] % nums[0] vào cuối, khi đó mảng trở thành [2,3,1], sau đó xóa các phần tử tại chỉ số 1 và 0.
nums trở thành [1].
Không thể giảm thêm độ dài của nums. Do đó, đáp án là 1.
Có thể chứng minh rằng 1 là độ dài nhỏ nhất có thể đạt được.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác thay thế hai số dương bằng một số dư và làm ngắn mảng đi một phần tử. Với $n \le 10^5$, ta không thể mô phỏng cho đến khi còn lại chỉ vài phần tử.
>
> Số dư luôn nhỏ hơn nghiêm ngặt so với toán hạng lớn hơn. Nếu có giá trị không chia hết cho giá trị nhỏ nhất toàn cục $\textit{mi}$, ta có thể tạo ra một số dương nhỏ hơn $\textit{mi}$ rồi xóa mọi phần tử khác, chỉ còn lại một phần tử, nên độ dài là $1$.
>
> Nếu mọi giá trị đều là bội của $\textit{mi}$, sẽ không xuất hiện số dương nào nhỏ hơn. Khi đó chỉ còn các bản sao của $\textit{mi}$, và ghép từng cặp sẽ để lại $\lceil \textit{cnt}/2 \rceil$ phần tử.

<!-- thinking:end -->

Gọi phần tử nhỏ nhất trong mảng $nums$ là $mi$.

Nếu $mi$ chỉ xuất hiện một lần, ta có thể thực hiện các thao tác với $mi$ và những phần tử khác trong mảng $nums$ để xóa tất cả các phần tử còn lại, chỉ giữ lại $mi$. Đáp án là $1$.

Nếu $mi$ xuất hiện nhiều lần, ta cần kiểm tra xem mọi phần tử trong mảng $nums$ có phải là bội của $mi$ hay không. Nếu không, tồn tại ít nhất một phần tử $x$ sao cho $0 < x \bmod mi < mi$. Điều này có nghĩa là ta có thể tạo ra một phần tử nhỏ hơn $mi$ thông qua các thao tác. Phần tử nhỏ hơn này có thể xóa tất cả các phần tử khác thông qua các thao tác, chỉ giữ lại chính nó. Đáp án là $1$. Nếu mọi phần tử đều là bội của $mi$, trước tiên ta có thể dùng $mi$ để xóa tất cả các phần tử lớn hơn $mi$. Các phần tử còn lại đều là $mi$, với số lượng là $cnt$. Ghép chúng thành từng cặp và thực hiện một thao tác cho mỗi cặp. Cuối cùng, còn lại $\lceil cnt / 2 \rceil$ phần tử, nên đáp án là $\lceil cnt / 2 \rceil$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumArrayLength(self, nums: List[int]) -> int:
        mi = min(nums)
        if any(x % mi for x in nums):
            return 1
        return (nums.count(mi) + 1) // 2
```

#### Java

```java
class Solution {
    public int minimumArrayLength(int[] nums) {
        int mi = Arrays.stream(nums).min().getAsInt();
        int cnt = 0;
        for (int x : nums) {
            if (x % mi != 0) {
                return 1;
            }
            if (x == mi) {
                ++cnt;
            }
        }
        return (cnt + 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumArrayLength(vector<int>& nums) {
        int mi = *min_element(nums.begin(), nums.end());
        int cnt = 0;
        for (int x : nums) {
            if (x % mi) {
                return 1;
            }
            cnt += x == mi;
        }
        return (cnt + 1) / 2;
    }
};
```

#### Go

```go
func minimumArrayLength(nums []int) int {
	mi := slices.Min(nums)
	cnt := 0
	for _, x := range nums {
		if x%mi != 0 {
			return 1
		}
		if x == mi {
			cnt++
		}
	}
	return (cnt + 1) / 2
}
```

#### TypeScript

```ts
function minimumArrayLength(nums: number[]): number {
    const mi = Math.min(...nums);
    let cnt = 0;
    for (const x of nums) {
        if (x % mi) {
            return 1;
        }
        if (x === mi) {
            ++cnt;
        }
    }
    return (cnt + 1) >> 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
