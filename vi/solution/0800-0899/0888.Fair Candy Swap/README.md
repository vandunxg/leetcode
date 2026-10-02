---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [888. Fair Candy Swap](https://leetcode.com/problems/fair-candy-swap)

[中文文档](/solution/0800-0899/0888.Fair%20Candy%20Swap/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob có tổng số kẹo khác nhau. Cho hai mảng số nguyên <code>aliceSizes</code> và <code>bobSizes</code>, trong đó <code>aliceSizes[i]</code> là số kẹo trong hộp thứ <code>i<sup>th</sup></code> của Alice, còn <code>bobSizes[j]</code> là số kẹo trong hộp thứ <code>j<sup>th</sup></code> của Bob.</p>

<p>Vì là bạn bè, họ muốn mỗi người đổi một hộp kẹo để sau khi trao đổi, tổng số kẹo của hai người bằng nhau. Tổng số kẹo một người có là tổng số kẹo trong tất cả các hộp của người đó.</p>

<p>Trả về <em>mảng số nguyên </em><code>answer</code><em>, trong đó </em><code>answer[0]</code><em> là số kẹo trong hộp Alice cần đổi, còn </em><code>answer[1]</code><em> là số kẹo trong hộp Bob cần đổi</em>. Nếu có nhiều đáp án, bạn có thể <strong>trả về bất kỳ đáp án nào</strong>. Đảm bảo luôn tồn tại ít nhất một đáp án.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> aliceSizes = [1,1], bobSizes = [2,2]
<strong>Đầu ra:</strong> [1,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> aliceSizes = [1,2], bobSizes = [2,3]
<strong>Đầu ra:</strong> [1,2]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> aliceSizes = [2], bobSizes = [1,3]
<strong>Đầu ra:</strong> [2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= aliceSizes.length, bobSizes.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= aliceSizes[i], bobSizes[j] &lt;= 10<sup>5</sup></code></li>
	<li>Alice và Bob có tổng số kẹo khác nhau.</li>
	<li>Đầu vào luôn có ít nhất một đáp án hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người chỉ đổi một hộp để tổng số kẹo bằng nhau. Nếu Alice đưa hộp có $a$ viên và Bob đưa hộp có $b$ viên, thì $a-b$ bằng một nửa chênh lệch tổng số kẹo. Vì $n\le 10^4$, duyệt toàn bộ hộp của Bob cho mỗi $a$ sẽ có độ phức tạp bậc hai.
>
> Lưu số kẹo trong các hộp của Bob vào set; với mỗi $a$, kiểm tra xem có hộp chứa $a-\textit{diff}$ hay không. Đảm bảo luôn có lời giải.

<!-- thinking:end -->

Trước tiên, ta tính chênh lệch tổng số kẹo giữa Alice và Bob rồi chia đôi để tìm chênh lệch số kẹo cần trao đổi $\textit{diff}$. Ta dùng hash table $\textit{s}$ để lưu số kẹo trong các hộp của Bob. Sau đó, duyệt các hộp của Alice; với mỗi số kẹo $\textit{a}$, ta kiểm tra xem $\textit{a} - \textit{diff}$ có trong hash table $\textit{s}$ hay không. Nếu có, ta đã tìm được đáp án hợp lệ và trả về đáp án đó.

Độ phức tạp thời gian là $O(m + n)$ và độ phức tạp không gian là $O(n)$, trong đó $m$ và $n$ lần lượt là số hộp kẹo của Alice và Bob.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fairCandySwap(self, aliceSizes: List[int], bobSizes: List[int]) -> List[int]:
        diff = (sum(aliceSizes) - sum(bobSizes)) >> 1
        s = set(bobSizes)
        for a in aliceSizes:
            if (b := (a - diff)) in s:
                return [a, b]
```

#### Java

```java
class Solution {
    public int[] fairCandySwap(int[] aliceSizes, int[] bobSizes) {
        int s1 = 0, s2 = 0;
        Set<Integer> s = new HashSet<>();
        for (int a : aliceSizes) {
            s1 += a;
        }
        for (int b : bobSizes) {
            s.add(b);
            s2 += b;
        }
        int diff = (s1 - s2) >> 1;
        for (int a : aliceSizes) {
            int b = a - diff;
            if (s.contains(b)) {
                return new int[] {a, b};
            }
        }
        return null;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> fairCandySwap(vector<int>& aliceSizes, vector<int>& bobSizes) {
        int s1 = accumulate(aliceSizes.begin(), aliceSizes.end(), 0);
        int s2 = accumulate(bobSizes.begin(), bobSizes.end(), 0);
        int diff = (s1 - s2) >> 1;
        unordered_set<int> s(bobSizes.begin(), bobSizes.end());
        vector<int> ans;
        for (int& a : aliceSizes) {
            int b = a - diff;
            if (s.count(b)) {
                ans = vector<int>{a, b};
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func fairCandySwap(aliceSizes []int, bobSizes []int) []int {
	s1, s2 := 0, 0
	s := map[int]bool{}
	for _, a := range aliceSizes {
		s1 += a
	}
	for _, b := range bobSizes {
		s2 += b
		s[b] = true
	}
	diff := (s1 - s2) / 2
	for _, a := range aliceSizes {
		if b := a - diff; s[b] {
			return []int{a, b}
		}
	}
	return nil
}
```

#### TypeScript

```ts
function fairCandySwap(aliceSizes: number[], bobSizes: number[]): number[] {
    const s1 = aliceSizes.reduce((acc, cur) => acc + cur, 0);
    const s2 = bobSizes.reduce((acc, cur) => acc + cur, 0);
    const diff = (s1 - s2) >> 1;
    const s = new Set(bobSizes);
    for (const a of aliceSizes) {
        const b = a - diff;
        if (s.has(b)) {
            return [a, b];
        }
    }
    return [];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
