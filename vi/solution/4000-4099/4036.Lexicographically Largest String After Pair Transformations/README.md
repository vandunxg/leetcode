---
comments: true
difficulty: Medium
rating: 1704
source: Biweekly Contest 190 Q3
---

<!-- problem:start -->

# [4036. Lexicographically Largest String After Pair Transformations](https://leetcode.com/problems/lexicographically-largest-string-after-pair-transformations)

[Tài liệu tiếng Trung](/solution/4000-4099/4036.Lexicographically%20Largest%20String%20After%20Pair%20Transformations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Với mỗi số nguyên <code>x</code> trong <code>nums</code>, ban đầu ta có một chuỗi gồm chính xác <code>x</code> ký tự <code>&#39;a&#39;</code> viết thường.</p>

<p>Bạn có thể thực hiện thao tác sau một số lần tùy ý (kể cả không lần nào):</p>

<ul>
	<li>Chọn hai chữ cái <strong>giống nhau và liền kề</strong>, rồi thay chúng bằng chữ cái tiếp theo trong bảng chữ cái.</li>
</ul>

<p>Ví dụ, có thể thay <code>&quot;aa&quot;</code> bằng <code>&quot;b&quot;</code>, và <code>&quot;bb&quot;</code> bằng <code>&quot;c&quot;</code>. Không thể thay cặp <code>&quot;zz&quot;</code>.</p>

<p>Với mỗi <code>x</code>, hãy xác định chuỗi <strong>lớn nhất theo thứ tự từ điển</strong> có thể nhận được.</p>

<p>Trả về một mảng các chuỗi, trong đó chuỗi thứ <code>i<sup>th</sup></code> là đáp án tương ứng với <code>nums[i]</code>.</p>

<p>Một chuỗi <code>a</code> được gọi là <strong>lớn hơn theo thứ tự từ điển</strong> chuỗi <code>b</code> nếu tại vị trí đầu tiên mà chúng khác nhau, <code>a</code> chứa một chữ cái xuất hiện sau chữ cái tương ứng trong <code>b</code> theo thứ tự bảng chữ cái. Nếu <code>min(a.length, b.length)</code> ký tự đầu tiên giống nhau, chuỗi dài hơn sẽ lớn hơn theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;b&quot;,&quot;ca&quot;,&quot;cba&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>nums[0] = 2</code>: <code>&quot;aa&quot;</code> &rarr; <code>&quot;b&quot;</code>.</li>
	<li><code>nums[1] = 5</code>: <code>&quot;aaaaa&quot;</code> &rarr; <code>&quot;baaa&quot;</code> &rarr; <code>&quot;bba&quot;</code> &rarr; <code>&quot;ca&quot;</code>.</li>
	<li><code>nums[2] = 7</code>: <code>&quot;aaaaaaa&quot;</code> &rarr; <code>&quot;baaaaa&quot;</code> &rarr; <code>&quot;bbaaa&quot;</code> &rarr; <code>&quot;bbba&quot;</code> &rarr; <code>&quot;cba&quot;</code>.</li>
	<li>Do đó, <code>ans = [&quot;b&quot;, &quot;ca&quot;, &quot;cba&quot;]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,9,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;ba&quot;,&quot;da&quot;,&quot;a&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>nums[0] = 3</code>: <code>&quot;aaa&quot;</code> &rarr; <code>&quot;ba&quot;</code>.</li>
	<li><code>nums[1] = 9</code>: <code>&quot;aaaaaaaaa&quot;</code> &rarr; <code>&quot;baaaaaaa&quot;</code> &rarr; <code>&quot;bbaaaaa&quot;</code> &rarr; <code>&quot;bbbaaa&quot;</code> &rarr; <code>&quot;bbbba&quot;</code> &rarr; <code>&quot;cbba&quot;</code> &rarr; <code>&quot;cca&quot;</code> &rarr; <code>&quot;da&quot;</code>.</li>
	<li><code>nums[2] = 1</code>: Không thể thực hiện phép biến đổi nào, nên kết quả là <code>&quot;a&quot;</code>.</li>
	<li>Do đó, <code>ans = [&quot;ba&quot;, &quot;da&quot;, &quot;a&quot;]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Phân rã nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Hai chữ cái giống nhau liền kề hợp thành chữ cái tiếp theo, nên $\texttt{'a'}+j$ tương đương với $2^j$ chữ cái $\texttt{'a'}$. Các chuỗi có thể tạo ra từ $x$ bản sao của $\texttt{'a'}$ chính xác là những chuỗi có tổng trọng số của các chữ cái bằng $x$.
>
> Chuỗi lớn nhất theo thứ tự từ điển sử dụng các chữ cái có trọng số lớn hơn trước. Với $j=25$ giảm dần, ta lấy $t=\lfloor x/2^j\rfloor$ bản sao của chữ cái đó rồi đặt $x\leftarrow x\bmod 2^j$.
>
> Với $j<25$ thì $t\in\{0,1\}$, vì vậy chỉ $\texttt{'z'}$ mới có thể lặp lại, và $\texttt{"zz"}$ không thể hợp thêm. Do đó chuỗi thu được là hợp lệ và lớn nhất.

<!-- thinking:end -->

Vì hai chữ cái giống hệt nhau liền kề sẽ hợp thành chữ cái tiếp theo trong bảng chữ cái, chữ cái $\texttt{'a'} + j$ tương đương với $2^j$ bản sao của $\texttt{'a'}$. Nói cách khác, các chuỗi có thể tạo ra từ $x$ bản sao của $\texttt{'a'}$ chính xác là những chuỗi có tổng trọng số các chữ cái bằng $x$.

Để tối đa hóa thứ tự từ điển, ta tham lam sử dụng các chữ cái có trọng số lớn nhất trước. Chữ cái lớn nhất là $\texttt{'z'}$ với trọng số $2^{25}$, nên ta duyệt $j$ từ $25$ xuống $0$, thêm $t = \left\lfloor x / 2^j \right\rfloor$ bản sao của chữ cái $\texttt{'a'} + j$ vào đáp án, rồi đặt $x \leftarrow x \bmod 2^j$.

Lưu ý rằng $t \in \{0, 1\}$ khi $j \lt 25$, nên chỉ $\texttt{'z'}$ có thể xuất hiện liên tiếp trong đáp án, và $\texttt{"zz"}$ không thể được hợp thêm. Vì vậy, chuỗi thu được là hợp lệ và lớn nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(\log M)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$ và $M$ là giá trị lớn nhất trong mảng $\textit{nums}$. Không tính phần không gian dành cho đáp án.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestString(self, nums: List[int]) -> List[str]:
        ans = []
        for x in nums:
            s = []
            for j in range(25, -1, -1):
                t = x >> j
                s.append(chr(ord('a') + j) * t)
                x &= (1 << j) - 1
            ans.append(''.join(s))
        return ans
```

#### Java

```java
class Solution {
    public String[] largestString(int[] nums) {
        int n = nums.length;
        String[] ans = new String[n];
        for (int k = 0; k < n; ++k) {
            int x = nums[k];
            StringBuilder s = new StringBuilder();
            for (int j = 25; j >= 0; --j) {
                for (int t = x >> j; t > 0; --t) {
                    s.append((char) ('a' + j));
                }
                x &= (1 << j) - 1;
            }
            ans[k] = s.toString();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> largestString(vector<int>& nums) {
        vector<string> ans;
        ans.reserve(nums.size());
        for (int x : nums) {
            string s;
            for (int j = 25; j >= 0; --j) {
                for (int t = x >> j; t > 0; --t) {
                    s.push_back('a' + j);
                }
                x &= (1 << j) - 1;
            }
            ans.push_back(s);
        }
        return ans;
    }
};
```

#### Go

```go
func largestString(nums []int) []string {
	ans := make([]string, 0, len(nums))
	for _, x := range nums {
		s := []byte{}
		for j := 25; j >= 0; j-- {
			for t := x >> j; t > 0; t-- {
				s = append(s, byte('a'+j))
			}
			x &= (1 << j) - 1
		}
		ans = append(ans, string(s))
	}
	return ans
}
```

#### TypeScript

```ts
function largestString(nums: number[]): string[] {
    const ans: string[] = [];
    for (let x of nums) {
        const s: string[] = [];
        for (let j = 25; j >= 0; --j) {
            const t = x >> j;
            s.push(String.fromCharCode(97 + j).repeat(t));
            x &= (1 << j) - 1;
        }
        ans.push(s.join(''));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
