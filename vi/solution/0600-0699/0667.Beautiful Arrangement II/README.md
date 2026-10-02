---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [667. Beautiful Arrangement II](https://leetcode.com/problems/beautiful-arrangement-ii)

[中文文档](/solution/0600-0699/0667.Beautiful%20Arrangement%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, hãy tạo danh sách <code>answer</code> gồm <code>n</code> số nguyên dương khác nhau trong khoảng từ <code>1</code> đến <code>n</code>, thỏa mãn yêu cầu sau:</p>

<ul>
	<li>Giả sử danh sách là <code>answer =&nbsp;[a<sub>1</sub>, a<sub>2</sub>, a<sub>3</sub>, ... , a<sub>n</sub>]</code>, khi đó danh sách <code>[|a<sub>1</sub> - a<sub>2</sub>|, |a<sub>2</sub> - a<sub>3</sub>|, |a<sub>3</sub> - a<sub>4</sub>|, ... , |a<sub>n-1</sub> - a<sub>n</sub>|]</code> có đúng <code>k</code> số nguyên phân biệt.</li>
</ul>

<p>Trả về <em>danh sách</em> <code>answer</code>. Nếu có nhiều đáp án hợp lệ, trả về <strong>bất kỳ đáp án nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 1
<strong>Đầu ra:</strong> [1,2,3]
Giải thích: [1,2,3] gồm ba số nguyên dương khác nhau trong khoảng từ 1 đến 3, còn [1,1] có đúng 1 số nguyên phân biệt là 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 2
<strong>Đầu ra:</strong> [1,3,2]
Giải thích: [1,3,2] gồm ba số nguyên dương khác nhau trong khoảng từ 1 đến 3, còn [2,1] có đúng 2 số nguyên phân biệt là 1 và 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt; n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị của $1..n$ cần có đúng $k$ hiệu tuyệt đối giữa các phần tử kề nhau khác nhau. Tìm kiếm ngẫu nhiên không đáng tin cậy.
>
> Xếp zigzag $1,n,2,n-1,\ldots$ cho $k$ phần tử đầu để tạo ra $k-1$ hiệu khác nhau; điền các phần tử còn lại theo thứ tự từ đầu trái hoặc đầu phải chưa dùng, sao cho không phát sinh hiệu mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def constructArray(self, n: int, k: int) -> List[int]:
        l, r = 1, n
        ans = []
        for i in range(k):
            if i % 2 == 0:
                ans.append(l)
                l += 1
            else:
                ans.append(r)
                r -= 1
        for i in range(k, n):
            if k % 2 == 0:
                ans.append(r)
                r -= 1
            else:
                ans.append(l)
                l += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] constructArray(int n, int k) {
        int l = 1, r = n;
        int[] ans = new int[n];
        for (int i = 0; i < k; ++i) {
            ans[i] = i % 2 == 0 ? l++ : r--;
        }
        for (int i = k; i < n; ++i) {
            ans[i] = k % 2 == 0 ? r-- : l++;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> constructArray(int n, int k) {
        int l = 1, r = n;
        vector<int> ans(n);
        for (int i = 0; i < k; ++i) {
            ans[i] = i % 2 == 0 ? l++ : r--;
        }
        for (int i = k; i < n; ++i) {
            ans[i] = k % 2 == 0 ? r-- : l++;
        }
        return ans;
    }
};
```

#### Go

```go
func constructArray(n int, k int) []int {
	l, r := 1, n
	ans := make([]int, n)
	for i := 0; i < k; i++ {
		if i%2 == 0 {
			ans[i] = l
			l++
		} else {
			ans[i] = r
			r--
		}
	}
	for i := k; i < n; i++ {
		if k%2 == 0 {
			ans[i] = r
			r--
		} else {
			ans[i] = l
			l++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function constructArray(n: number, k: number): number[] {
    let l = 1;
    let r = n;
    const ans = new Array(n);
    for (let i = 0; i < k; ++i) {
        ans[i] = i % 2 == 0 ? l++ : r--;
    }
    for (let i = k; i < n; ++i) {
        ans[i] = k % 2 == 0 ? r-- : l++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
