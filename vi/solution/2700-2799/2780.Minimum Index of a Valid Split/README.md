---
comments: true
difficulty: Medium
rating: 1549
source: Weekly Contest 354 Q3
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2780. Minimum Index of a Valid Split](https://leetcode.com/problems/minimum-index-of-a-valid-split)

[中文文档](/solution/2700-2799/2780.Minimum%20Index%20of%20a%20Valid%20Split/README.md)

## Mô tả

<!-- description:start -->

<p>Một phần tử <code>x</code> của mảng số nguyên <code>arr</code> có độ dài <code>m</code> được gọi là <strong>chiếm ưu thế</strong> nếu <strong>hơn một nửa</strong> số phần tử trong <code>arr</code> có giá trị là <code>x</code>.</p>

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code> và có một phần tử <strong>chiếm ưu thế</strong>.</p>

<p>Bạn có thể chia <code>nums</code> tại chỉ số <code>i</code> thành hai mảng <code>nums[0, ..., i]</code> và <code>nums[i + 1, ..., n - 1]</code>, nhưng phép chia chỉ <strong>hợp lệ</strong> khi:</p>

<ul>
	<li><code>0 &lt;= i &lt; n - 1</code></li>
	<li><code>nums[0, ..., i]</code> và <code>nums[i + 1, ..., n - 1]</code> có cùng phần tử chiếm ưu thế.</li>
</ul>

<p>Ở đây, <code>nums[i, ..., j]</code> biểu thị mảng con của <code>nums</code> bắt đầu tại chỉ số <code>i</code> và kết thúc tại chỉ số <code>j</code>, bao gồm cả hai đầu. Cụ thể, nếu <code>j &lt; i</code> thì <code>nums[i, ..., j]</code> biểu thị một mảng con rỗng.</p>

<p>Trả về <em>chỉ số <strong>nhỏ nhất</strong> của một <strong>phép chia hợp lệ</strong></em>. Nếu không tồn tại phép chia hợp lệ, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chia mảng tại chỉ số 2 để thu được hai mảng [1,2,2] và [2].
Trong mảng [1,2,2], phần tử 2 chiếm ưu thế vì nó xuất hiện hai lần trong mảng và 2 * 2 &gt; 3.
Trong mảng [2], phần tử 2 chiếm ưu thế vì nó xuất hiện một lần trong mảng và 1 * 2 &gt; 1.
Cả [1,2,2] và [2] đều có cùng phần tử chiếm ưu thế với nums, nên đây là một phép chia hợp lệ.
Có thể chứng minh rằng chỉ số 2 là chỉ số nhỏ nhất của một phép chia hợp lệ. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,1,1,1,7,1,2,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể chia mảng tại chỉ số 4 để thu được hai mảng [2,1,3,1,1] và [1,7,1,2,1].
Trong mảng [2,1,3,1,1], phần tử 1 chiếm ưu thế vì nó xuất hiện ba lần trong mảng và 3 * 2 &gt; 5.
Trong mảng [1,7,1,2,1], phần tử 1 chiếm ưu thế vì nó xuất hiện ba lần trong mảng và 3 * 2 &gt; 5.
Cả [2,1,3,1,1] và [1,7,1,2,1] đều có cùng phần tử chiếm ưu thế với nums, nên đây là một phép chia hợp lệ.
Có thể chứng minh rằng chỉ số 4 là chỉ số nhỏ nhất của một phép chia hợp lệ.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3,7,2,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không tồn tại phép chia hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> có đúng một phần tử chiếm ưu thế.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một phép chia hợp lệ cần có cùng phần tử chiếm ưu thế ở cả hai phía. Giá trị chiếm ưu thế xuất hiện nhiều hơn một nửa số lần và nếu tồn tại thì là duy nhất trong toàn bộ mảng, vì vậy hai phía đều phải sử dụng cùng giá trị đó.
>
> Đếm để tìm giá trị xuất hiện nhiều nhất $x$ trong toàn bộ mảng và số lần xuất hiện của nó, sau đó duyệt số lần xuất hiện của $x$ trong tiền tố rồi trả về chỉ số đầu tiên mà cả hai phía đều có $x$ chiếm đa số nghiêm ngặt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumIndex(self, nums: List[int]) -> int:
        x, cnt = Counter(nums).most_common(1)[0]
        cur = 0
        for i, v in enumerate(nums, 1):
            if v == x:
                cur += 1
                if cur * 2 > i and (cnt - cur) * 2 > len(nums) - i:
                    return i - 1
        return -1
```

#### Java

```java
class Solution {
    public int minimumIndex(List<Integer> nums) {
        int x = 0, cnt = 0;
        Map<Integer, Integer> freq = new HashMap<>();
        for (int v : nums) {
            int t = freq.merge(v, 1, Integer::sum);
            if (cnt < t) {
                cnt = t;
                x = v;
            }
        }
        int cur = 0;
        for (int i = 1; i <= nums.size(); ++i) {
            if (nums.get(i - 1) == x) {
                ++cur;
                if (cur * 2 > i && (cnt - cur) * 2 > nums.size() - i) {
                    return i - 1;
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumIndex(vector<int>& nums) {
        int x = 0, cnt = 0;
        unordered_map<int, int> freq;
        for (int v : nums) {
            ++freq[v];
            if (freq[v] > cnt) {
                cnt = freq[v];
                x = v;
            }
        }
        int cur = 0;
        for (int i = 1; i <= nums.size(); ++i) {
            if (nums[i - 1] == x) {
                ++cur;
                if (cur * 2 > i && (cnt - cur) * 2 > nums.size() - i) {
                    return i - 1;
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumIndex(nums []int) int {
	x, cnt := 0, 0
	freq := map[int]int{}
	for _, v := range nums {
		freq[v]++
		if freq[v] > cnt {
			x, cnt = v, freq[v]
		}
	}
	cur := 0
	for i, v := range nums {
		i++
		if v == x {
			cur++
			if cur*2 > i && (cnt-cur)*2 > len(nums)-i {
				return i - 1
			}
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumIndex(nums: number[]): number {
    let [x, cnt] = [0, 0];
    const freq: Map<number, number> = new Map();
    for (const v of nums) {
        freq.set(v, (freq.get(v) ?? 0) + 1);
        if (freq.get(v)! > cnt) {
            [x, cnt] = [v, freq.get(v)!];
        }
    }
    let cur = 0;
    for (let i = 1; i <= nums.length; ++i) {
        if (nums[i - 1] === x) {
            ++cur;
            if (cur * 2 > i && (cnt - cur) * 2 > nums.length - i) {
                return i - 1;
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
