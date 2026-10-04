---
comments: true
difficulty: Medium
rating: 1613
source: Biweekly Contest 108 Q2
tags:
    - Array
    - Hash Table
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [2766. Relocate Marbles](https://leetcode.com/problems/relocate-marbles)

[中文文档](/solution/2700-2799/2766.Relocate%20Marbles/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn vị trí ban đầu của một số viên bi. Bạn cũng được cho hai mảng số nguyên <code>moveFrom</code> và <code>moveTo</code> được đánh chỉ số từ <strong>0</strong> và có độ dài <strong>bằng nhau</strong>.</p>

<p>Trong <code>moveFrom.length</code> bước, bạn sẽ thay đổi vị trí của các viên bi. Ở bước thứ <code>i<sup>th</sup></code>, bạn sẽ chuyển <strong>tất cả</strong> viên bi ở vị trí <code>moveFrom[i]</code> đến vị trí <code>moveTo[i]</code>.</p>

<p>Sau khi hoàn thành tất cả các bước, hãy trả về <em>danh sách đã sắp xếp các vị trí <strong>đang có viên bi</strong></em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Một vị trí được gọi là <strong>đang có viên bi</strong> nếu có ít nhất một viên bi ở vị trí đó.</li>
	<li>Một vị trí có thể chứa nhiều viên bi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,6,7,8], moveFrom = [1,7,2], moveTo = [2,9,5]
<strong>Đầu ra:</strong> [5,6,8,9]
<strong>Giải thích:</strong> Ban đầu, các viên bi ở các vị trí 1,6,7,8.
Ở bước i = 0, ta chuyển các viên bi ở vị trí 1 đến vị trí 2. Khi đó, các vị trí 2,6,7,8 đang có viên bi.
Ở bước i = 1, ta chuyển các viên bi ở vị trí 7 đến vị trí 9. Khi đó, các vị trí 2,6,8,9 đang có viên bi.
Ở bước i = 2, ta chuyển các viên bi ở vị trí 2 đến vị trí 5. Khi đó, các vị trí 5,6,8,9 đang có viên bi.
Cuối cùng, các vị trí chứa ít nhất một viên bi là [5,6,8,9].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,3,3], moveFrom = [1,3], moveTo = [2,2]
<strong>Đầu ra:</strong> [2]
<strong>Giải thích:</strong> Ban đầu, các viên bi ở các vị trí [1,1,3,3].
Ở bước i = 0, ta chuyển tất cả các viên bi ở vị trí 1 đến vị trí 2. Khi đó, các viên bi ở các vị trí [2,2,3,3].
Ở bước i = 1, ta chuyển tất cả các viên bi ở vị trí 3 đến vị trí 2. Khi đó, các viên bi ở các vị trí [2,2,2,2].
Vì 2 là vị trí duy nhất đang có viên bi, ta trả về [2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= moveFrom.length &lt;= 10<sup>5</sup></code></li>
	<li><code>moveFrom.length == moveTo.length</code></li>
	<li><code>1 &lt;= nums[i], moveFrom[i], moveTo[i] &lt;= 10<sup>9</sup></code></li>
	<li>Các test được tạo sao cho tại thời điểm thực hiện bước di chuyển thứ <code>i<sup>th</sup></code>, luôn có ít nhất một viên bi ở <code>moveFrom[i]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần di chuyển sẽ chuyển mọi viên bi từ một vị trí này sang vị trí khác; mục tiêu là tìm các vị trí đang có viên bi sau cùng. Việc duyệt mảng sẽ tốn kém nếu một vị trí có nhiều viên bi.
>
> Ta lưu các tọa độ đang có viên bi trong một set: xóa vị trí nguồn rồi thêm vị trí đích. Cuối cùng, sắp xếp set để thu được đáp án.

<!-- thinking:end -->

Ta dùng một hash table $pos$ để lưu tất cả vị trí của các viên bi. Ban đầu, $pos$ chứa tất cả phần tử của $nums$. Sau đó, ta duyệt qua $moveFrom$ và $moveTo$. Ở mỗi bước, ta xóa $moveFrom[i]$ khỏi $pos$ và thêm $moveTo[i]$ vào $pos$. Cuối cùng, ta sắp xếp các phần tử trong $pos$ rồi trả về.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def relocateMarbles(
        self, nums: List[int], moveFrom: List[int], moveTo: List[int]
    ) -> List[int]:
        pos = set(nums)
        for f, t in zip(moveFrom, moveTo):
            pos.remove(f)
            pos.add(t)
        return sorted(pos)
```

#### Java

```java
class Solution {
    public List<Integer> relocateMarbles(int[] nums, int[] moveFrom, int[] moveTo) {
        Set<Integer> pos = new HashSet<>();
        for (int x : nums) {
            pos.add(x);
        }
        for (int i = 0; i < moveFrom.length; ++i) {
            pos.remove(moveFrom[i]);
            pos.add(moveTo[i]);
        }
        List<Integer> ans = new ArrayList<>(pos);
        ans.sort((a, b) -> a - b);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> relocateMarbles(vector<int>& nums, vector<int>& moveFrom, vector<int>& moveTo) {
        unordered_set<int> pos(nums.begin(), nums.end());
        for (int i = 0; i < moveFrom.size(); ++i) {
            pos.erase(moveFrom[i]);
            pos.insert(moveTo[i]);
        }
        vector<int> ans(pos.begin(), pos.end());
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func relocateMarbles(nums []int, moveFrom []int, moveTo []int) (ans []int) {
	pos := map[int]bool{}
	for _, x := range nums {
		pos[x] = true
	}
	for i, f := range moveFrom {
		t := moveTo[i]
		pos[f] = false
		pos[t] = true
	}
	for x, ok := range pos {
		if ok {
			ans = append(ans, x)
		}
	}
	sort.Ints(ans)
	return
}
```

#### TypeScript

```ts
function relocateMarbles(nums: number[], moveFrom: number[], moveTo: number[]): number[] {
    const pos: Set<number> = new Set(nums);
    for (let i = 0; i < moveFrom.length; i++) {
        pos.delete(moveFrom[i]);
        pos.add(moveTo[i]);
    }
    return [...pos].sort((a, b) => a - b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
