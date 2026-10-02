---
comments: true
difficulty: Medium
rating: 1701
source: Weekly Contest 138 Q4
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1054. Distant Barcodes](https://leetcode.com/problems/distant-barcodes)

[中文文档](/solution/1000-1099/1054.Distant%20Barcodes/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một nhà kho có một dãy mã vạch, trong đó mã vạch thứ <code>i<sup>th</sup></code> là <code>barcodes[i]</code>.</p>

<p>Hãy sắp xếp lại các mã vạch sao cho không có hai mã liền kề nào giống nhau. Bạn có thể trả về bất kỳ đáp án nào; đảm bảo luôn tồn tại đáp án.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> barcodes = [1,1,1,2,2,2]
<strong>Đầu ra:</strong> [2,1,2,1,2,1]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> barcodes = [1,1,1,1,2,2,3,3]
<strong>Đầu ra:</strong> [1,3,1,3,1,2,1,2]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= barcodes.length &lt;= 10000</code></li>
	<li><code>1 &lt;= barcodes[i] &lt;= 10000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm và sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị liền kề phải khác nhau. Nếu giá trị xuất hiện nhiều nhất có số lần xuất hiện không quá $\lceil n/2\rceil$, luôn có cách sắp xếp hợp lệ. Đặt các giá trị xuất hiện nhiều vào những chỉ số chẵn trước rồi điền các chỉ số lẻ để giữ chúng cách nhau.
>
> Sắp xếp theo tần suất giảm dần, rồi theo giá trị tăng dần để các số giống nhau đứng cạnh nhau và số xuất hiện nhiều được xếp trước. Ghi nửa đầu vào các vị trí chẵn, phần còn lại vào các vị trí lẻ.
>
> Giá trị xuất hiện nhiều nhất chiếm các vị trí cách nhau một ô và không bao giờ đứng cạnh chính nó.

<!-- thinking:end -->

Trước tiên, dùng hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của mỗi giá trị trong mảng $barcodes$. Sau đó, sắp xếp các giá trị trong $barcodes$ theo số lần xuất hiện trong $cnt$ từ nhiều đến ít. Nếu số lần xuất hiện bằng nhau, sắp xếp theo giá trị tăng dần (để các giá trị giống nhau đứng cạnh nhau).

Tiếp theo, tạo mảng kết quả $ans$ có độ dài $n$. Duyệt mảng $barcodes$ đã sắp xếp và lần lượt điền các phần tử vào những chỉ số chẵn $0, 2, 4, \cdots$ của $ans$. Sau đó, điền các phần tử còn lại vào những chỉ số lẻ $1, 3, 5, \cdots$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài mảng $barcodes$ và $M$ là giá trị lớn nhất trong mảng này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeBarcodes(self, barcodes: List[int]) -> List[int]:
        cnt = Counter(barcodes)
        barcodes.sort(key=lambda x: (-cnt[x], x))
        n = len(barcodes)
        ans = [0] * len(barcodes)
        ans[::2] = barcodes[: (n + 1) // 2]
        ans[1::2] = barcodes[(n + 1) // 2 :]
        return ans
```

#### Java

```java
class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {
        int n = barcodes.length;
        Integer[] t = new Integer[n];
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            t[i] = barcodes[i];
            mx = Math.max(mx, barcodes[i]);
        }
        int[] cnt = new int[mx + 1];
        for (int x : barcodes) {
            ++cnt[x];
        }
        Arrays.sort(t, (a, b) -> cnt[a] == cnt[b] ? a - b : cnt[b] - cnt[a]);
        int[] ans = new int[n];
        for (int k = 0, j = 0; k < 2; ++k) {
            for (int i = k; i < n; i += 2) {
                ans[i] = t[j++];
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
    vector<int> rearrangeBarcodes(vector<int>& barcodes) {
        int mx = *max_element(barcodes.begin(), barcodes.end());
        int cnt[mx + 1];
        memset(cnt, 0, sizeof(cnt));
        for (int x : barcodes) {
            ++cnt[x];
        }
        sort(barcodes.begin(), barcodes.end(), [&](int a, int b) {
            return cnt[a] > cnt[b] || (cnt[a] == cnt[b] && a < b);
        });
        int n = barcodes.size();
        vector<int> ans(n);
        for (int k = 0, j = 0; k < 2; ++k) {
            for (int i = k; i < n; i += 2) {
                ans[i] = barcodes[j++];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rearrangeBarcodes(barcodes []int) []int {
	mx := slices.Max(barcodes)
	cnt := make([]int, mx+1)
	for _, x := range barcodes {
		cnt[x]++
	}
	sort.Slice(barcodes, func(i, j int) bool {
		a, b := barcodes[i], barcodes[j]
		if cnt[a] == cnt[b] {
			return a < b
		}
		return cnt[a] > cnt[b]
	})
	n := len(barcodes)
	ans := make([]int, n)
	for k, j := 0, 0; k < 2; k++ {
		for i := k; i < n; i, j = i+2, j+1 {
			ans[i] = barcodes[j]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rearrangeBarcodes(barcodes: number[]): number[] {
    const mx = Math.max(...barcodes);
    const cnt = Array(mx + 1).fill(0);
    for (const x of barcodes) {
        ++cnt[x];
    }
    barcodes.sort((a, b) => (cnt[a] === cnt[b] ? a - b : cnt[b] - cnt[a]));
    const n = barcodes.length;
    const ans = Array(n);
    for (let k = 0, j = 0; k < 2; ++k) {
        for (let i = k; i < n; i += 2, ++j) {
            ans[i] = barcodes[j];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
