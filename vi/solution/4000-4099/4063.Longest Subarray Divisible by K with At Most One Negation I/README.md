---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [4063. Longest Subarray Divisible by K with At Most One Negation I](https://leetcode.com/problems/longest-subarray-divisible-by-k-with-at-most-one-negation-i)

[中文文档](/solution/4000-4099/4063.Longest%20Subarray%20Divisible%20by%20K%20with%20At%20Most%20One%20Negation%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một mảng con là <strong>hợp lệ</strong> nếu tổng của nó chia hết cho <code>k</code>, hoặc có thể trở thành số chia hết cho <code>k</code> bằng cách <strong>đổi dấu một phần tử trong mảng con đó</strong>.</p>

<p>Đổi dấu một phần tử có nghĩa là thay giá trị <code>x</code> bằng <code>-x</code>.</p>

<p>Trả về <strong>độ dài của mảng con hợp lệ dài nhất</strong>. Nếu không tồn tại mảng con hợp lệ, trả về 0.</p>

<p>Một <strong>mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,2], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của toàn bộ mảng là 7, và <code>7 % 3 = 1</code>, nên nó không chia hết cho <code>k = 3</code>.</li>
	<li>Đổi dấu <code>nums[2] = 2</code> làm tổng trở thành <code>4 + 1 &minus; 2 = 3</code>, chia hết cho <code>k</code>.</li>
	<li>Do đó, toàn bộ mảng là một mảng con hợp lệ, có độ dài bằng 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,3,4], k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của toàn bộ mảng là 12, và đổi dấu bất kỳ phần tử nào cũng không làm tổng chia hết cho 7.</li>
	<li>Tuy nhiên, mảng con <code>[3, 4]</code> có tổng bằng 7, chia hết cho <code>k = 7</code> mà không cần đổi dấu.</li>
	<li>Do đó, mảng con hợp lệ dài nhất có độ dài bằng 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,5], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của toàn bộ mảng là 9, và đổi dấu bất kỳ phần tử nào cũng không làm tổng chia hết cho 6.</li>
	<li>Mảng con <code>[2, 2]</code> có tổng bằng 4. Đổi dấu một trong hai phần tử làm nó trở thành <code>[-2, 2]</code> hoặc <code>[2, -2]</code>, cả hai đều có tổng bằng 0.</li>
	<li>Do đó, mảng con hợp lệ dài nhất có độ dài bằng 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê chỉ số bị đổi dấu

<!-- thinking:start -->

> **Tư duy**
>
> Tổng của một mảng con chia hết cho $k$ khi và chỉ khi tổng tiền tố tại hai đầu của nó đồng dư modulo $k$. Liệt kê mọi mảng con rồi mọi phần tử bên trong nó có độ phức tạp khoảng $O(n^3)$. Vì $n\le 1000$, cần giảm độ phức tạp đi một bậc.
>
> Đổi dấu $x$ làm giảm tổng của mọi mảng con chứa nó đi $2x$, còn các mảng con khác không bị ảnh hưởng. Có $n$ lựa chọn chỉ số bị đổi dấu, cùng với lựa chọn không đổi dấu phần tử nào. Với mỗi lựa chọn, chỉ cần tìm mảng con dài nhất có tổng chia hết cho $k$.
>
> Với mỗi số dư modulo $k$ của tổng tiền tố, lưu chỉ số đầu tiên mà nó xuất hiện. Khi gặp lại số dư đó, đầu mảng con ở vị trí trước đó sẽ cho một mảng con dài hơn. Mảng con không chứa chỉ số bị đổi dấu đã được tính trên mảng ban đầu.

<!-- thinking:end -->

Sau khi đổi dấu $\textit{nums}[i]$, mọi mảng con chứa chỉ số $i$ đều giảm tổng đi $2\times\textit{nums}[i]$, còn mọi mảng con không chứa $i$ vẫn giữ nguyên tổng. Chạy quá trình tìm mảng con chia hết cho k trên mảng ban đầu, sau đó chạy lại khi lần lượt đổi dấu từng chỉ số. Đáp án là độ dài lớn nhất trong các trường hợp đó.

Trong quá trình duyệt, lưu tổng tiền tố modulo $k$, với số dư được đưa về đoạn $[0, k)$. Với mỗi số dư, lưu chỉ số đầu tiên mà nó xuất hiện; số dư $0$ bắt đầu tại chỉ số $-1$. Tại chỉ số $i$, nếu số dư hiện tại đã xuất hiện ở chỉ số $j$, thì tổng của $\textit{nums}[j+1..i]$ chia hết cho $k$ và độ dài là $i-j$. Python, Java, Go và TypeScript lưu các chỉ số này trong một hash map. C++ sử dụng một mảng có độ dài $k$, được truy cập bằng số dư, và truyền chỉ số cần đổi dấu làm tham số.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. C++ đặt lại mảng sau mỗi lần duyệt, nên độ phức tạp thời gian là $O(n(n+k))$ và độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: list[int], k: int) -> int:
        def f(nums: list[int], k: int) -> int:
            d = {0: -1}
            s = res = 0
            for i, x in enumerate(nums):
                s = (s + x) % k
                if s in d:
                    res = max(res, i - d[s])
                else:
                    d[s] = i
            return res

        ans = f(nums, k)
        for i, x in enumerate(nums):
            nums[i] = -x
            ans = max(ans, f(nums, k))
            nums[i] = x
        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums, int k) {
        int ans = f(nums, k);
        for (int i = 0; i < nums.length; ++i) {
            nums[i] = -nums[i];
            ans = Math.max(ans, f(nums, k));
            nums[i] = -nums[i];
        }
        return ans;
    }

    private int f(int[] nums, int k) {
        Map<Integer, Integer> d = new HashMap<>();
        d.put(0, -1);
        int s = 0, res = 0;
        for (int i = 0; i < nums.length; ++i) {
            s = ((s + nums[i]) % k + k) % k;
            if (d.containsKey(s)) {
                res = Math.max(res, i - d.get(s));
            } else {
                d.put(s, i);
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> d(k, -2);

        auto f = [&](int skip) {
            fill(d.begin(), d.end(), -2);
            d[0] = -1;
            int s = 0, res = 0;
            for (int i = 0; i < n; ++i) {
                int x = i == skip ? -nums[i] : nums[i];
                s = (s + x) % k;
                if (s < 0) {
                    s += k;
                }
                if (d[s] != -2) {
                    res = max(res, i - d[s]);
                } else {
                    d[s] = i;
                }
            }
            return res;
        };

        int ans = f(-1);
        for (int i = 0; i < n; ++i) {
            ans = max(ans, f(i));
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int, k int) int {
	f := func(nums []int) int {
		d := map[int]int{0: -1}
		s, res := 0, 0
		for i, x := range nums {
			s = (s + x) % k
			if s < 0 {
				s += k
			}
			if j, ok := d[s]; ok {
				res = max(res, i-j)
			} else {
				d[s] = i
			}
		}
		return res
	}

	ans := f(nums)
	for i, x := range nums {
		nums[i] = -x
		ans = max(ans, f(nums))
		nums[i] = x
	}
	return ans
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[], k: number): number {
    const f = (nums: number[]): number => {
        const d = new Map<number, number>([[0, -1]]);
        let s = 0,
            res = 0;
        for (let i = 0; i < nums.length; ++i) {
            s = (s + nums[i]) % k;
            if (s < 0) {
                s += k;
            }
            if (d.has(s)) {
                res = Math.max(res, i - d.get(s)!);
            } else {
                d.set(s, i);
            }
        }
        return res;
    };

    let ans = f(nums);
    for (let i = 0; i < nums.length; ++i) {
        nums[i] = -nums[i];
        ans = Math.max(ans, f(nums));
        nums[i] = -nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
