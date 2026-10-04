---
comments: true
difficulty: Easy
rating: 1189
source: Weekly Contest 374 Q1
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [2951. Find the Peaks](https://leetcode.com/problems/find-the-peaks)

[中文文档](/solution/2900-2999/2951.Find%20the%20Peaks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>được đánh chỉ số từ 0</strong> <code>mountain</code>. Nhiệm vụ của bạn là tìm tất cả các <strong>đỉnh</strong> trong mảng <code>mountain</code>.</p>

<p>Trả về <em>một mảng gồm các</em> chỉ số<!-- notionvc: c9879de8-88bd-43b0-8224-40c4bee71cd6 --><em> của các <strong>đỉnh</strong> trong mảng đã cho theo <strong>bất kỳ thứ tự nào</strong>.</em></p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Một <strong>đỉnh</strong> được định nghĩa là một phần tử <strong>lớn hơn nghiêm ngặt</strong> các phần tử kề nó.</li>
	<li>Phần tử đầu tiên và cuối cùng của mảng <strong>không</strong> phải là đỉnh.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mountain = [2,4,4]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> mountain[0] và mountain[2] không thể là đỉnh vì chúng là phần tử đầu tiên và cuối cùng của mảng.
mountain[1] cũng không thể là đỉnh vì nó không lớn hơn nghiêm ngặt mountain[2].
Vì vậy, đáp án là [].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mountain = [1,4,3,8,5]
<strong>Đầu ra:</strong> [1,3]
<strong>Giải thích:</strong> mountain[0] và mountain[4] không thể là đỉnh vì chúng là phần tử đầu tiên và cuối cùng của mảng.
mountain[2] cũng không thể là đỉnh vì nó không lớn hơn nghiêm ngặt mountain[3] và mountain[1].
Nhưng mountain [1] và mountain[3] lớn hơn nghiêm ngặt các phần tử kề chúng.
Vì vậy, đáp án là [1,3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= mountain.length &lt;= 100</code></li>
	<li><code>1 &lt;= mountain[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Một đỉnh là một chỉ số ở giữa, có giá trị lớn hơn nghiêm ngặt cả hai phần tử kề nó. Vì $n \le 100$, chỉ cần duyệt từ $1$ đến $n-2$ và so sánh từng bộ ba phần tử.
>
> Theo định nghĩa, hai đầu mảng không bao giờ là đỉnh.

<!-- thinking:end -->

Ta duyệt trực tiếp các chỉ số $i \in [1, n-2]$. Với mỗi chỉ số $i$, nếu $mountain[i-1] < mountain[i]$ và $mountain[i + 1] < mountain[i]$, thì $mountain[i]$ là một đỉnh, và ta thêm chỉ số $i$ vào mảng đáp án.

Sau khi kết thúc việc duyệt, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Không tính phần không gian dùng cho mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPeaks(self, mountain: List[int]) -> List[int]:
        return [
            i
            for i in range(1, len(mountain) - 1)
            if mountain[i - 1] < mountain[i] > mountain[i + 1]
        ]
```

#### Java

```java
class Solution {
    public List<Integer> findPeaks(int[] mountain) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 1; i < mountain.length - 1; ++i) {
            if (mountain[i - 1] < mountain[i] && mountain[i + 1] < mountain[i]) {
                ans.add(i);
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
    vector<int> findPeaks(vector<int>& mountain) {
        vector<int> ans;
        for (int i = 1; i < mountain.size() - 1; ++i) {
            if (mountain[i - 1] < mountain[i] && mountain[i + 1] < mountain[i]) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findPeaks(mountain []int) (ans []int) {
	for i := 1; i < len(mountain)-1; i++ {
		if mountain[i-1] < mountain[i] && mountain[i+1] < mountain[i] {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function findPeaks(mountain: number[]): number[] {
    const ans: number[] = [];
    for (let i = 1; i < mountain.length - 1; ++i) {
        if (mountain[i - 1] < mountain[i] && mountain[i + 1] < mountain[i]) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
