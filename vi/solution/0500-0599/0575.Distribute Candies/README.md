---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [575. Distribute Candies](https://leetcode.com/problems/distribute-candies)

[中文文档](/solution/0500-0599/0575.Distribute%20Candies/README.md)

## Mô tả

<!-- description:start -->

<p>Alice có <code>n</code> viên kẹo, trong đó viên thứ <code>i</code> thuộc loại <code>candyType[i]</code>. Alice nhận thấy mình bắt đầu tăng cân nên đã đi khám bác sĩ.</p>

<p>Bác sĩ khuyên Alice chỉ nên ăn <code>n / 2</code> viên trong số kẹo cô có (<code>n</code> luôn là số chẵn). Alice rất thích kẹo và muốn ăn nhiều loại kẹo khác nhau nhất có thể mà vẫn làm theo lời khuyên của bác sĩ.</p>

<p>Cho mảng số nguyên <code>candyType</code> có độ dài <code>n</code>, hãy trả về <em><strong>số lượng lớn nhất</strong> các loại kẹo khác nhau Alice có thể ăn nếu chỉ ăn </em><code>n / 2</code><em> viên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candyType = [1,1,2,2,3,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Alice chỉ có thể ăn 6 / 2 = 3 viên kẹo. Vì chỉ có 3 loại nên cô có thể ăn mỗi loại một viên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candyType = [1,1,2,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Alice chỉ có thể ăn 4 / 2 = 2 viên kẹo. Dù chọn các loại [1,2], [1,3] hay [2,3], cô cũng chỉ ăn được 2 loại khác nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> candyType = [6,6,6,6]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Alice chỉ có thể ăn 4 / 2 = 2 viên kẹo. Dù được ăn 2 viên, cô chỉ có một loại kẹo.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == candyType.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>n</code> là số chẵn.</li>
	<li><code>-10<sup>5</sup> &lt;= candyType[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Alice chỉ được ăn một nửa số kẹo và muốn chọn nhiều loại nhất có thể. Số loại vượt quá $n/2$ sẽ không thể ăn hết.
>
> Số loại khác nhau chính là kích thước của set; đáp án là $\min(\textit{types},\ n/2)$. Không cần mô phỏng việc chia kẹo.

<!-- thinking:end -->

Ta dùng hash table để lưu các loại kẹo. Nếu số loại kẹo ít hơn $n / 2$, Alice có thể ăn mỗi loại một viên và số loại tối đa bằng tổng số loại. Ngược lại, cô chỉ có thể ăn tối đa $n / 2$ loại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số viên kẹo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distributeCandies(self, candyType: List[int]) -> int:
        return min(len(candyType) >> 1, len(set(candyType)))
```

#### Java

```java
class Solution {
    public int distributeCandies(int[] candyType) {
        Set<Integer> s = new HashSet<>();
        for (int c : candyType) {
            s.add(c);
        }
        return Math.min(candyType.length >> 1, s.size());
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distributeCandies(vector<int>& candyType) {
        unordered_set<int> s(candyType.begin(), candyType.end());
        return min(candyType.size() >> 1, s.size());
    }
};
```

#### Go

```go
func distributeCandies(candyType []int) int {
	s := hashset.New()
	for _, c := range candyType {
		s.Add(c)
	}
	return min(len(candyType)>>1, s.Size())
}
```

#### TypeScript

```ts
function distributeCandies(candyType: number[]): number {
    const s = new Set(candyType);
    return Math.min(s.size, candyType.length >> 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
