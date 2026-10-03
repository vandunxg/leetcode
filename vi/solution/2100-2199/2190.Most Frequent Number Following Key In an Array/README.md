---
comments: true
difficulty: Easy
rating: 1289
source: Biweekly Contest 73 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2190. Most Frequent Number Following Key In an Array](https://leetcode.com/problems/most-frequent-number-following-key-in-an-array)

[中文文档](/solution/2100-2199/2190.Most%20Frequent%20Number%20Following%20Key%20In%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code>.<strong> </strong>Đồng thời cho một số nguyên <code>key</code>, và key xuất hiện trong <code>nums</code>.</p>

<p>Với mỗi số nguyên khác nhau <code>target</code> trong <code>nums</code>, hãy <strong>đếm</strong> số lần <code>target</code> xuất hiện ngay sau một lần xuất hiện của <code>key</code> trong <code>nums</code>. Nói cách khác, hãy đếm số chỉ số <code>i</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt;= nums.length - 2</code>,</li>
	<li><code>nums[i] == key</code> và</li>
	<li><code>nums[i + 1] == target</code>.</li>
</ul>

<p>Trả về <em>giá trị </em><code>target</code><em> có số lần xuất hiện <strong>lớn nhất</strong></em>. Các test được tạo sao cho <code>target</code> có số lần xuất hiện lớn nhất là duy nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,100,200,1,100], key = 1
<strong>Đầu ra:</strong> 100
<strong>Giải thích:</strong> Với target = 100, có 2 lần xuất hiện tại các chỉ số 1 và 4, ngay sau một lần xuất hiện của key.
Không có số nguyên nào khác xuất hiện sau một lần xuất hiện của key, nên ta trả về 100.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,2,3], key = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Với target = 2, có 3 lần xuất hiện tại các chỉ số 1, 2 và 3, ngay sau một lần xuất hiện của key.
Với target = 3, chỉ có 1 lần xuất hiện tại chỉ số 4, ngay sau một lần xuất hiện của key.
target = 2 có số lần xuất hiện sau một lần xuất hiện của key lớn nhất, nên ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li>Các test được tạo sao cho đáp án là duy nhất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các giá trị xuất hiện ngay sau $\textit{key}$ và trả về giá trị có tần suất cao nhất duy nhất. Chỉ cần duyệt một lần qua các cặp phần tử liền kề.
>
> Mỗi khi giá trị bên trái của một cặp bằng $\textit{key}$, tăng số đếm của giá trị bên phải và ghi nhớ kết quả tốt nhất hiện tại.
>
> Hash table được giới hạn bởi miền giá trị.

<!-- thinking:end -->

Ta sử dụng một hash table hoặc một mảng $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi $\textit{target}$, đồng thời dùng biến $\textit{mx}$ để duy trì số lần xuất hiện lớn nhất của $\textit{target}$. Ban đầu, $\textit{mx} = 0$.

Duyệt qua mảng $\textit{nums}$. Nếu $\textit{nums}[i] = \textit{key}$, tăng số đếm của $\textit{nums}[i + 1]$ trong $\textit{cnt}[\textit{nums}[i + 1]]$. Nếu $\textit{mx} \lt \textit{cnt}[\textit{nums}[i + 1]]$, cập nhật $\textit{mx} = \textit{cnt}[\textit{nums}[i + 1]]$ và cập nhật đáp án $\textit{ans} = \textit{nums}[i + 1]$.

Sau khi duyệt xong, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(M)$. Trong đó, $n$ và $M$ lần lượt là độ dài của mảng $\textit{nums}$ và giá trị lớn nhất của các phần tử trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostFrequent(self, nums: List[int], key: int) -> int:
        cnt = Counter()
        ans = mx = 0
        for a, b in pairwise(nums):
            if a == key:
                cnt[b] += 1
                if mx < cnt[b]:
                    mx = cnt[b]
                    ans = b
        return ans
```

#### Java

```java
class Solution {
    public int mostFrequent(int[] nums, int key) {
        int[] cnt = new int[1001];
        int ans = 0, mx = 0;
        for (int i = 0; i < nums.length - 1; ++i) {
            if (nums[i] == key) {
                if (mx < ++cnt[nums[i + 1]]) {
                    mx = cnt[nums[i + 1]];
                    ans = nums[i + 1];
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
    int mostFrequent(vector<int>& nums, int key) {
        int cnt[1001]{};
        int ans = 0, mx = 0;
        for (int i = 0; i < nums.size() - 1; ++i) {
            if (nums[i] == key) {
                if (mx < ++cnt[nums[i + 1]]) {
                    mx = cnt[nums[i + 1]];
                    ans = nums[i + 1];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mostFrequent(nums []int, key int) (ans int) {
	cnt := [1001]int{}
	mx := 0
	for i, x := range nums[1:] {
		if nums[i] == key {
			cnt[x]++
			if mx < cnt[x] {
				mx = cnt[x]
				ans = x
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function mostFrequent(nums: number[], key: number): number {
    const cnt: number[] = Array(Math.max(...nums) + 1).fill(0);
    let [ans, mx] = [0, 0];
    for (let i = 0; i < nums.length - 1; ++i) {
        if (nums[i] === key) {
            if (mx < ++cnt[nums[i + 1]]) {
                mx = cnt[nums[i + 1]];
                ans = nums[i + 1];
            }
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} key
 * @return {number}
 */
var mostFrequent = function (nums, key) {
    const cnt = Array(Math.max(...nums) + 1).fill(0);
    let [ans, mx] = [0, 0];
    for (let i = 0; i < nums.length - 1; ++i) {
        if (nums[i] === key) {
            if (mx < ++cnt[nums[i + 1]]) {
                mx = cnt[nums[i + 1]];
                ans = nums[i + 1];
            }
        }
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    function mostFrequent($nums, $key) {
        $cnt = array_fill(0, max($nums) + 1, 0);
        $ans = 0;
        $mx = 0;
        for ($i = 0; $i < count($nums) - 1; ++$i) {
            if ($nums[$i] === $key) {
                $cnt[$nums[$i + 1]]++;
                if ($mx < $cnt[$nums[$i + 1]]) {
                    $mx = $cnt[$nums[$i + 1]];
                    $ans = $nums[$i + 1];
                }
            }
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
