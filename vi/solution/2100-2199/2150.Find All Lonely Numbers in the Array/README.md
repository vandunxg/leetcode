---
comments: true
difficulty: Medium
rating: 1275
source: Weekly Contest 277 Q3
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2150. Find All Lonely Numbers in the Array](https://leetcode.com/problems/find-all-lonely-numbers-in-the-array)

[中文文档](/solution/2100-2199/2150.Find%20All%20Lonely%20Numbers%20in%20the%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Một số <code>x</code> là <strong>cô độc</strong> khi nó chỉ xuất hiện <strong>một lần</strong>, và không có số nào <strong>kề</strong> với nó (tức là <code>x + 1</code> và <code>x - 1)</code>) xuất hiện trong mảng.</p>

<p>Trả về <em><strong>tất cả</strong> các số cô độc trong </em><code>nums</code>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,6,5,8]
<strong>Đầu ra:</strong> [10,8]
<strong>Giải thích:</strong>
- 10 là một số cô độc vì nó xuất hiện đúng một lần và 9 cũng như 11 không xuất hiện trong nums.
- 8 là một số cô độc vì nó xuất hiện đúng một lần và 7 cũng như 9 không xuất hiện trong nums.
- 5 không phải là một số cô độc vì 6 xuất hiện trong nums và ngược lại.
Do đó, các số cô độc trong nums là [10, 8].
Lưu ý rằng cũng có thể trả về [8, 10].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,3]
<strong>Đầu ra:</strong> [1,5]
<strong>Giải thích:</strong>
- 1 là một số cô độc vì nó xuất hiện đúng một lần và 0 cũng như 2 không xuất hiện trong nums.
- 5 là một số cô độc vì nó xuất hiện đúng một lần và 4 cũng như 6 không xuất hiện trong nums.
- 3 không phải là một số cô độc vì nó xuất hiện hai lần.
Do đó, các số cô độc trong nums là [1, 5].
Lưu ý rằng cũng có thể trả về [5, 1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một số cô độc xuất hiện đúng một lần và không có giá trị lân cận nào xuất hiện. Nếu sắp xếp rồi kiểm tra các phần tử kề nhau, ta vẫn phải xử lý các giá trị trùng; một bảng tần suất có thể kiểm tra cả ba điều kiện cùng lúc.
>
> Đếm số lần xuất hiện, sau đó giữ lại các khóa thỏa mãn $v=1$ và không có $x-1$ hoặc $x+1$.
>
> Độ phức tạp thời gian và bộ nhớ bổ sung đều là $O(n)$.

<!-- thinking:end -->

Ta sử dụng một bảng băm $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi số. Sau đó, ta duyệt qua bảng băm. Với mỗi số và số lần xuất hiện tương ứng $(x, v)$, nếu $v = 1$ và $\textit{cnt}[x - 1] = 0$ và $\textit{cnt}[x + 1] = 0$, thì $x$ là một số cô độc, và ta thêm nó vào mảng đáp án.

Sau khi duyệt xong, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLonely(self, nums: List[int]) -> List[int]:
        cnt = Counter(nums)
        return [
            x for x, v in cnt.items() if v == 1 and cnt[x - 1] == 0 and cnt[x + 1] == 0
        ]
```

#### Java

```java
class Solution {
    public List<Integer> findLonely(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        List<Integer> ans = new ArrayList<>();
        for (var e : cnt.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            if (v == 1 && !cnt.containsKey(x - 1) && !cnt.containsKey(x + 1)) {
                ans.add(x);
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
    vector<int> findLonely(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            cnt[x]++;
        }
        vector<int> ans;
        for (auto& [x, v] : cnt) {
            if (v == 1 && !cnt.contains(x - 1) && !cnt.contains(x + 1)) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLonely(nums []int) (ans []int) {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for x, v := range cnt {
		if v == 1 && cnt[x-1] == 0 && cnt[x+1] == 0 {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function findLonely(nums: number[]): number[] {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    const ans: number[] = [];
    for (const [x, v] of cnt) {
        if (v === 1 && !cnt.has(x - 1) && !cnt.has(x + 1)) {
            ans.push(x);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
