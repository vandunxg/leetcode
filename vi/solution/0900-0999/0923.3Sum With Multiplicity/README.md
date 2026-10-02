---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Counting
    - Sorting
---

<!-- problem:start -->

# [923. 3Sum With Multiplicity](https://leetcode.com/problems/3sum-with-multiplicity)

[中文文档](/solution/0900-0999/0923.3Sum%20With%20Multiplicity/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> và số nguyên <code>target</code>, hãy trả về số bộ ba chỉ số <code>i, j, k</code> sao cho <code>i &lt; j &lt; k</code> và <code>arr[i] + arr[j] + arr[k] == target</code>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,1,2,2,3,3,4,4,5,5], target = 8
<strong>Đầu ra:</strong> 20
<strong>Giải thích: </strong>
Xét theo các giá trị (arr[i], arr[j], arr[k]):
(1, 2, 5) xuất hiện 8 lần;
(1, 3, 4) xuất hiện 8 lần;
(2, 2, 4) xuất hiện 2 lần;
(2, 3, 3) xuất hiện 2 lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,1,2,2,2,2], target = 5
<strong>Đầu ra:</strong> 12
<strong>Giải thích: </strong>
arr[i] = 1, arr[j] = arr[k] = 2 xuất hiện 12 lần:
Có 2 cách chọn một số 1 từ [1,1],
và 6 cách chọn hai số 2 từ [2,2,2,2].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,1,3], target = 6
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> (1, 2, 3) xuất hiện một lần trong mảng nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 3000</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 100</code></li>
	<li><code>0 &lt;= target &lt;= 300</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba có tổng bằng $target$. Các giá trị thuộc $[0,100]$ và $n\le 3000$, nên vét cạn bậc ba sẽ quá tốn kém. Đếm tần suất từng giá trị, sau đó duyệt các cặp $i<j$; giá trị thứ ba $c=target-a-b$ phải nằm sau $j$.
>
> Giảm tần suất của $b$ trước khi tra cứu $c$ để bộ đếm chỉ biểu diễn phần hậu tố; mỗi lần tra cứu mất $O(1)$.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $cnt$ có độ dài $101$ để đếm số lần xuất hiện của từng phần tử trong mảng $arr$.

Sau đó, với mỗi phần tử $arr[j]$ trong mảng $arr$, trước tiên giảm $cnt[arr[j]]$ đi $1$, rồi duyệt các phần tử $arr[i]$ đứng trước $arr[j]$ và tính $c = target - arr[i] - arr[j]$. Nếu $c$ thuộc đoạn $[0, 100]$, cộng $cnt[c]$ vào đáp án. Cuối cùng, trả về đáp án.

Lưu ý đáp án có thể vượt quá ${10}^9 + 7$, vì vậy hãy lấy modulo sau mỗi lần cộng.

Độ phức tạp thời gian là $O(n^2)$, với $n$ là độ dài mảng $arr$. Độ phức tạp không gian là $O(C)$, trong đó $C$ là giá trị lớn nhất của các phần tử trong mảng $arr$; ở bài này, $C = 100$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def threeSumMulti(self, arr: List[int], target: int) -> int:
        mod = 10**9 + 7
        cnt = Counter(arr)
        ans = 0
        for j, b in enumerate(arr):
            cnt[b] -= 1
            for a in arr[:j]:
                c = target - a - b
                ans = (ans + cnt[c]) % mod
        return ans
```

#### Java

```java
class Solution {
    public int threeSumMulti(int[] arr, int target) {
        final int mod = (int) 1e9 + 7;
        int[] cnt = new int[101];
        for (int x : arr) {
            ++cnt[x];
        }
        int n = arr.length;
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            --cnt[arr[j]];
            for (int i = 0; i < j; ++i) {
                int c = target - arr[i] - arr[j];
                if (c >= 0 && c < cnt.length) {
                    ans = (ans + cnt[c]) % mod;
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
    int threeSumMulti(vector<int>& arr, int target) {
        const int mod = 1e9 + 7;
        int cnt[101]{};
        for (int x : arr) {
            ++cnt[x];
        }
        int n = arr.size();
        int ans = 0;
        for (int j = 0; j < n; ++j) {
            --cnt[arr[j]];
            for (int i = 0; i < j; ++i) {
                int c = target - arr[i] - arr[j];
                if (c >= 0 && c <= 100) {
                    ans = (ans + cnt[c]) % mod;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func threeSumMulti(arr []int, target int) (ans int) {
	const mod int = 1e9 + 7
	cnt := [101]int{}
	for _, x := range arr {
		cnt[x]++
	}
	for j, b := range arr {
		cnt[b]--
		for _, a := range arr[:j] {
			if c := target - a - b; c >= 0 && c < len(cnt) {
				ans = (ans + cnt[c]) % mod
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function threeSumMulti(arr: number[], target: number): number {
    const mod = 10 ** 9 + 7;
    const cnt: number[] = Array(101).fill(0);
    for (const x of arr) {
        ++cnt[x];
    }
    let ans = 0;
    const n = arr.length;
    for (let j = 0; j < n; ++j) {
        --cnt[arr[j]];
        for (let i = 0; i < j; ++i) {
            const c = target - arr[i] - arr[j];
            if (c >= 0 && c < cnt.length) {
                ans = (ans + cnt[c]) % mod;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
