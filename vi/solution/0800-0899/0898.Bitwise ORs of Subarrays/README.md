---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [898. Bitwise ORs of Subarrays](https://leetcode.com/problems/bitwise-ors-of-subarrays)

[中文文档](/solution/0800-0899/0898.Bitwise%20ORs%20of%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, trả về <em>số kết quả OR theo bit khác nhau của tất cả mảng con không rỗng trong</em> <code>arr</code>.</p>

<p>OR theo bit của một mảng con là kết quả OR theo bit của tất cả số nguyên trong mảng con đó. Nếu mảng con chỉ có một số nguyên thì kết quả chính là số đó.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp và không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [0]
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Chỉ có một kết quả có thể là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,1,2]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Các mảng con có thể là [1], [1], [2], [1, 1], [1, 2], [1, 1, 2].
Các mảng con này cho kết quả lần lượt là 1, 1, 2, 1, 3, 3.
Có 3 giá trị khác nhau, nên đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,4]
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Các kết quả có thể là 1, 2, 3, 4, 6 và 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số kết quả OR theo bit khác nhau của mọi mảng con. Vì $n\le 5\cdot 10^4$, không thể liệt kê tất cả mảng con. Tập kết quả OR của các mảng con kết thúc tại $i$ gồm $\{y\lor arr[i]\}$ với mọi giá trị $y$ trong tập trước, cộng thêm $\{arr[i]\}$.
>
> Phép OR chỉ bật thêm các bit, nên kích thước tập này là $O(\log A)$. Cập nhật tập qua từng vị trí bằng hash set rồi hợp các kết quả vào một tập tổng; kích thước tập tổng chính là đáp án.

<!-- thinking:end -->

Bài toán yêu cầu đếm số kết quả OR theo bit khác nhau của các mảng con. Nếu xét lần lượt vị trí kết thúc $i$, số kết quả OR theo bit của các mảng con kết thúc tại $i-1$ không vượt quá $32$, vì phép OR theo bit không làm giảm giá trị.

Vì vậy, ta dùng hash table $ans$ để lưu mọi kết quả OR theo bit của các mảng con, và hash table $s$ để lưu kết quả của những mảng con kết thúc tại phần tử hiện tại. Ban đầu, $s$ chỉ chứa phần tử $0$.

Tiếp theo, ta lần lượt xét vị trí kết thúc $i$ của mảng con. Tập kết quả OR theo bit của các mảng con kết thúc tại $i$ được tạo bằng cách OR các kết quả của mảng con kết thúc tại $i-1$ với $a[i]$, đồng thời thêm chính $a[i]$. Ta dùng hash table $t$ để lưu các kết quả này, sau đó cập nhật $s = t$ và thêm mọi phần tử trong $t$ vào $ans$.

Cuối cùng, trả về số phần tử trong hash table $ans$.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(n \times \log M)$, trong đó $n$ là độ dài mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarrayBitwiseORs(self, arr: List[int]) -> int:
        ans = set()
        s = set()
        for x in arr:
            s = {x | y for y in s} | {x}
            ans |= s
        return len(ans)
```

#### Java

```java
class Solution {
    public int subarrayBitwiseORs(int[] arr) {
        Set<Integer> ans = new HashSet<>();
        Set<Integer> s = new HashSet<>();
        for (int x : arr) {
            Set<Integer> t = new HashSet<>();
            for (int y : s) {
                t.add(x | y);
            }
            t.add(x);
            ans.addAll(t);
            s = t;
        }
        return ans.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarrayBitwiseORs(vector<int>& arr) {
        unordered_set<int> ans;
        unordered_set<int> s;
        for (int x : arr) {
            unordered_set<int> t;
            for (int y : s) {
                t.insert(x | y);
            }
            t.insert(x);
            ans.insert(t.begin(), t.end());
            s = move(t);
        }
        return ans.size();
    }
};
```

#### Go

```go
func subarrayBitwiseORs(arr []int) int {
	ans := map[int]bool{}
	s := map[int]bool{}
	for _, x := range arr {
		t := map[int]bool{x: true}
		for y := range s {
			t[x|y] = true
		}
		for y := range t {
			ans[y] = true
		}
		s = t
	}
	return len(ans)
}
```

#### TypeScript

```ts
function subarrayBitwiseORs(arr: number[]): number {
    const ans: Set<number> = new Set();
    const s: Set<number> = new Set();
    for (const x of arr) {
        const t: Set<number> = new Set([x]);
        for (const y of s) {
            t.add(x | y);
        }
        s.clear();
        for (const y of t) {
            ans.add(y);
            s.add(y);
        }
    }
    return ans.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
