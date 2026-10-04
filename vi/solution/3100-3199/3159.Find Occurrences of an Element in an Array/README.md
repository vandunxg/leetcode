---
comments: true
difficulty: Medium
rating: 1262
source: Biweekly Contest 131 Q2
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3159. Find Occurrences of an Element in an Array](https://leetcode.com/problems/find-occurrences-of-an-element-in-an-array)

[中文文档](/solution/3100-3199/3159.Find%20Occurrences%20of%20an%20Element%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>, một mảng số nguyên <code>queries</code> và một số nguyên <code>x</code>.</p>

<p>Với mỗi <code>queries[i]</code>, bạn cần tìm chỉ số của lần xuất hiện thứ <code>queries[i]<sup>th</sup></code> của <code>x</code> trong mảng <code>nums</code>. Nếu số lần xuất hiện của <code>x</code> ít hơn <code>queries[i]</code>, câu trả lời cho truy vấn đó là -1.</p>

<p>Trả về một mảng số nguyên <code>answer</code> chứa câu trả lời cho tất cả các truy vấn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,1,7], queries = [1,3,2,4], x = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,-1,2,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với truy vấn thứ 1<sup>st</sup>, lần xuất hiện đầu tiên của 1 nằm ở chỉ số 0.</li>
	<li>Với truy vấn thứ 2<sup>nd</sup>, chỉ có hai lần xuất hiện của 1 trong <code>nums</code>, nên câu trả lời là -1.</li>
	<li>Với truy vấn thứ 3<sup>rd</sup>, lần xuất hiện thứ hai của 1 nằm ở chỉ số 2.</li>
	<li>Với truy vấn thứ 4<sup>th</sup>, chỉ có hai lần xuất hiện của 1 trong <code>nums</code>, nên câu trả lời là -1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], queries = [10], x = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với truy vấn thứ 1<sup>st</sup>, 5 không tồn tại trong <code>nums</code>, nên câu trả lời là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], x &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các truy vấn yêu cầu chỉ số của lần xuất hiện thứ $i$ của $x$. Nếu duyệt từ trái sang phải cho từng truy vấn thì độ phức tạp là $O(nm)$.
>
> Tất cả các chỉ số xuất hiện có thể được lưu trong một mảng $ids$, sau đó mỗi truy vấn chỉ cần một lần tra cứu.
>
> Thu thập các chỉ số của $x$, rồi trả về $ids[i-1]$ hoặc $-1$ khi $i$ quá lớn.

<!-- thinking:end -->

Theo mô tả bài toán, trước tiên ta có thể duyệt qua mảng `nums` để tìm chỉ số của tất cả phần tử có giá trị bằng $x$, rồi lưu chúng vào mảng `ids`.

Tiếp theo, ta duyệt qua mảng `queries`. Với mỗi truy vấn $i$, nếu $i - 1$ nhỏ hơn độ dài của `ids` thì câu trả lời là `ids[i - 1]`, ngược lại câu trả lời là $-1$.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng `nums` và `queries`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def occurrencesOfElement(
        self, nums: List[int], queries: List[int], x: int
    ) -> List[int]:
        ids = [i for i, v in enumerate(nums) if v == x]
        return [ids[i - 1] if i - 1 < len(ids) else -1 for i in queries]
```

#### Java

```java
class Solution {
    public int[] occurrencesOfElement(int[] nums, int[] queries, int x) {
        List<Integer> ids = new ArrayList<>();
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] == x) {
                ids.add(i);
            }
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int j = queries[i] - 1;
            ans[i] = j < ids.size() ? ids.get(j) : -1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> occurrencesOfElement(vector<int>& nums, vector<int>& queries, int x) {
        vector<int> ids;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == x) {
                ids.push_back(i);
            }
        }
        vector<int> ans;
        for (int& i : queries) {
            ans.push_back(i - 1 < ids.size() ? ids[i - 1] : -1);
        }
        return ans;
    }
};
```

#### Go

```go
func occurrencesOfElement(nums []int, queries []int, x int) (ans []int) {
	ids := []int{}
	for i, v := range nums {
		if v == x {
			ids = append(ids, i)
		}
	}
	for _, i := range queries {
		if i-1 < len(ids) {
			ans = append(ans, ids[i-1])
		} else {
			ans = append(ans, -1)
		}
	}
	return
}
```

#### TypeScript

```ts
function occurrencesOfElement(nums: number[], queries: number[], x: number): number[] {
    const ids: number[] = nums.map((v, i) => (v === x ? i : -1)).filter(v => v !== -1);
    return queries.map(i => ids[i - 1] ?? -1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
