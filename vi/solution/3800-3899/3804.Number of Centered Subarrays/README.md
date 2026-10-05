---
comments: true
difficulty: Medium
rating: 1393
source: Weekly Contest 484 Q2
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [3804. Number of Centered Subarrays](https://leetcode.com/problems/number-of-centered-subarrays)

[中文文档](/solution/3800-3899/3804.Number%20of%20Centered%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> của <code>nums</code> được gọi là <strong>centered</strong> nếu tổng các phần tử của nó <strong>bằng với ít nhất một</strong> phần tử trong <strong>chính mảng con đó</strong>.</p>

<p>Hãy trả về số lượng <strong>mảng con centered</strong> của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tất cả các mảng con chỉ gồm một phần tử (<code>[-1]</code>, <code>[1]</code>, <code>[0]</code>) đều là mảng con centered.</li>
	<li>Mảng con <code>[1, 0]</code> có tổng bằng 1, là giá trị xuất hiện trong mảng con.</li>
	<li>Mảng con <code>[-1, 1, 0]</code> có tổng bằng 0, là giá trị xuất hiện trong mảng con.</li>
	<li>Vì vậy, đáp án là 5.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,-3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ các mảng con chỉ gồm một phần tử (<code>[2]</code>, <code>[-3]</code>) là mảng con centered.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 500</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con centered có tổng bằng một trong các phần tử của nó. Với $n \le 500$, dùng ba vòng lặp kèm tìm kiếm tuyến tính sẽ nặng hơn cần thiết.
>
> Cố định đầu trái và mở rộng về bên phải cho phép cập nhật tăng dần cả tổng và tập các giá trị.
>
> Ta duyệt start $i$, duy trì tổng hiện tại $s$ và hash set của $nums[i..j]$, rồi đếm khi $s$ xuất hiện trong tập.
>
> Tất cả các mảng con $O(n^2)$ đều được duyệt; phép kiểm tra hash có thời gian hằng số trung bình.

<!-- thinking:end -->

Ta duyệt tất cả các chỉ số bắt đầu $i$ của mảng con, sau đó bắt đầu từ chỉ số $i$, ta duyệt chỉ số kết thúc $j$ của mảng con, tính tổng $s$ của các phần tử trong mảng con $nums[i \ldots j]$, và thêm tất cả các phần tử trong mảng con vào hash table $\textit{st}$. Sau mỗi lần duyệt, ta kiểm tra xem $s$ có xuất hiện trong hash table $\textit{st}$ hay không. Nếu có, điều đó có nghĩa là mảng con $nums[i \ldots j]$ là một mảng con centered, và ta tăng đáp án thêm $1$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def centeredSubarrays(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            st = set()
            s = 0
            for j in range(i, n):
                s += nums[j]
                st.add(nums[j])
                if s in st:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int centeredSubarrays(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            Set<Integer> st = new HashSet<>();
            int s = 0;
            for (int j = i; j < n; j++) {
                s += nums[j];
                st.add(nums[j]);
                if (st.contains(s)) {
                    ans++;
                }
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
    int centeredSubarrays(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            unordered_set<int> st;
            int s = 0;
            for (int j = i; j < n; j++) {
                s += nums[j];
                st.insert(nums[j]);
                if (st.count(s)) {
                    ans++;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func centeredSubarrays(nums []int) int {
	n := len(nums)
	ans := 0
	for i := 0; i < n; i++ {
		st := make(map[int]struct{})
		s := 0
		for j := i; j < n; j++ {
			s += nums[j]
			st[nums[j]] = struct{}{}
			if _, ok := st[s]; ok {
				ans++
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function centeredSubarrays(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; i++) {
        const st = new Set<number>();
        let s = 0;
        for (let j = i; j < n; j++) {
            s += nums[j];
            st.add(nums[j]);
            if (st.has(s)) {
                ans++;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
