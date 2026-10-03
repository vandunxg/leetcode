---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [1852. Distinct Numbers in Each Subarray 🔒](https://leetcode.com/problems/distinct-numbers-in-each-subarray)

[中文文档](/solution/1800-1899/1852.Distinct%20Numbers%20in%20Each%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>. Nhiệm vụ của bạn là tìm số lượng phần tử <strong>phân biệt</strong> trong <strong>mọi</strong> mảng con có kích thước <code>k</code> của <code>nums</code>.</p>

<p>Trả về một mảng <code>ans</code> sao cho <code>ans[i]</code> là số lượng phần tử phân biệt trong <code>nums[i..(i + k - 1)]</code> với mỗi chỉ số <code>0 &lt;= i &lt; n - k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,2,2,1,3], k = 3
<strong>Đầu ra:</strong> [3,2,2,2,3]
<strong>Giải thích: </strong>Số lượng phần tử phân biệt trong mỗi mảng con được tính như sau:
- nums[0..2] = [1,2,3] nên ans[0] = 3
- nums[1..3] = [2,3,2] nên ans[1] = 2
- nums[2..4] = [3,2,2] nên ans[2] = 2
- nums[3..5] = [2,2,1] nên ans[3] = 2
- nums[4..6] = [2,1,3] nên ans[4] = 3
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,2,3,4], k = 4
<strong>Đầu ra:</strong> [1,2,3,4]
<strong>Giải thích: </strong>Số lượng phần tử phân biệt trong mỗi mảng con được tính như sau:
- nums[0..3] = [1,1,1,1] nên ans[0] = 1
- nums[1..4] = [1,1,1,2] nên ans[1] = 2
- nums[2..5] = [1,1,2,3] nên ans[2] = 3
- nums[3..6] = [1,2,3,4] nên ans[3] = 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần số lượng giá trị phân biệt trong mọi cửa sổ có độ dài $k$. Việc xây dựng lại một set cho từng cửa sổ quá chậm khi $n\le 10^5$.
>
> Duy trì frequency map của cửa sổ: tăng khi một phần tử đi vào, giảm khi một phần tử rời đi và xóa các key có giá trị bằng $0$. Kích thước của map chính là số lượng phần tử phân biệt; chỉ cần trượt qua mảng một lần.

<!-- thinking:end -->

Ta dùng một hash table $cnt$ để ghi lại số lần xuất hiện của mỗi số trong mảng con có độ dài $k$.

Trước tiên, ta duyệt $k$ phần tử đầu tiên của mảng, ghi lại số lần xuất hiện của từng phần tử. Sau khi duyệt xong, kích thước của hash table là phần tử đầu tiên của mảng đáp án.

Sau đó, ta tiếp tục duyệt mảng từ chỉ số $k$. Ở mỗi bước, ta tăng số lần xuất hiện của phần tử hiện tại lên một và giảm số lần xuất hiện của phần tử ở bên trái phần tử hiện tại xuống một. Nếu số lần xuất hiện của phần tử bên trái trở thành $0$ sau khi giảm, ta xóa nó khỏi hash table. Tiếp theo, kích thước của hash table là phần tử tiếp theo của mảng đáp án, rồi tiếp tục duyệt.

Sau khi duyệt xong, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(k)$, trong đó $n$ là độ dài của mảng $nums$ và $k$ là tham số được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctNumbers(self, nums: List[int], k: int) -> List[int]:
        cnt = Counter(nums[:k])
        ans = [len(cnt)]
        for i in range(k, len(nums)):
            cnt[nums[i]] += 1
            cnt[nums[i - k]] -= 1
            if cnt[nums[i - k]] == 0:
                cnt.pop(nums[i - k])
            ans.append(len(cnt))
        return ans
```

#### Java

```java
class Solution {
    public int[] distinctNumbers(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int i = 0; i < k; ++i) {
            cnt.merge(nums[i], 1, Integer::sum);
        }
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        ans[0] = cnt.size();
        for (int i = k; i < n; ++i) {
            cnt.merge(nums[i], 1, Integer::sum);
            if (cnt.merge(nums[i - k], -1, Integer::sum) == 0) {
                cnt.remove(nums[i - k]);
            }
            ans[i - k + 1] = cnt.size();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> distinctNumbers(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        for (int i = 0; i < k; ++i) {
            ++cnt[nums[i]];
        }
        int n = nums.size();
        vector<int> ans;
        ans.push_back(cnt.size());
        for (int i = k; i < n; ++i) {
            ++cnt[nums[i]];
            if (--cnt[nums[i - k]] == 0) {
                cnt.erase(nums[i - k]);
            }
            ans.push_back(cnt.size());
        }
        return ans;
    }
};
```

#### Go

```go
func distinctNumbers(nums []int, k int) []int {
	cnt := map[int]int{}
	for _, x := range nums[:k] {
		cnt[x]++
	}
	ans := []int{len(cnt)}
	for i := k; i < len(nums); i++ {
		cnt[nums[i]]++
		cnt[nums[i-k]]--
		if cnt[nums[i-k]] == 0 {
			delete(cnt, nums[i-k])
		}
		ans = append(ans, len(cnt))
	}
	return ans
}
```

#### TypeScript

```ts
function distinctNumbers(nums: number[], k: number): number[] {
    const cnt: Map<number, number> = new Map();
    for (let i = 0; i < k; ++i) {
        cnt.set(nums[i], (cnt.get(nums[i]) ?? 0) + 1);
    }
    const ans: number[] = [cnt.size];
    for (let i = k; i < nums.length; ++i) {
        cnt.set(nums[i], (cnt.get(nums[i]) ?? 0) + 1);
        cnt.set(nums[i - k], cnt.get(nums[i - k])! - 1);
        if (cnt.get(nums[i - k]) === 0) {
            cnt.delete(nums[i - k]);
        }
        ans.push(cnt.size);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding Window + Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng hash map với hằng số lớn hơn. Vì các giá trị không vượt quá $10^5$, ta có thể thay map bằng một mảng. Các thao tác cập nhật cửa sổ vẫn giữ nguyên; chỉ có chỉ số của số lần xuất hiện được thay bằng chính giá trị đó.

<!-- thinking:end -->

Ta cũng có thể dùng một mảng để thay thế hash table, giúp cải thiện hiệu năng ở một mức độ nhất định.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài của mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$. Trong bài này, $M \leq 10^5$.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int[] distinctNumbers(int[] nums, int k) {
        int m = 0;
        for (int x : nums) {
            m = Math.max(m, x);
        }
        int[] cnt = new int[m + 1];
        int v = 0;
        for (int i = 0; i < k; ++i) {
            if (++cnt[nums[i]] == 1) {
                ++v;
            }
        }
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        ans[0] = v;
        for (int i = k; i < n; ++i) {
            if (++cnt[nums[i]] == 1) {
                ++v;
            }
            if (--cnt[nums[i - k]] == 0) {
                --v;
            }
            ans[i - k + 1] = v;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> distinctNumbers(vector<int>& nums, int k) {
        int m = *max_element(begin(nums), end(nums));
        int cnt[m + 1];
        memset(cnt, 0, sizeof(cnt));
        int n = nums.size();
        int v = 0;
        vector<int> ans(n - k + 1);
        for (int i = 0; i < k; ++i) {
            if (++cnt[nums[i]] == 1) {
                ++v;
            }
        }
        ans[0] = v;
        for (int i = k; i < n; ++i) {
            if (++cnt[nums[i]] == 1) {
                ++v;
            }
            if (--cnt[nums[i - k]] == 0) {
                --v;
            }
            ans[i - k + 1] = v;
        }
        return ans;
    }
};
```

#### Go

```go
func distinctNumbers(nums []int, k int) (ans []int) {
	m := slices.Max(nums)
	cnt := make([]int, m+1)
	v := 0
	for _, x := range nums[:k] {
		cnt[x]++
		if cnt[x] == 1 {
			v++
		}
	}
	ans = append(ans, v)
	for i := k; i < len(nums); i++ {
		cnt[nums[i]]++
		if cnt[nums[i]] == 1 {
			v++
		}
		cnt[nums[i-k]]--
		if cnt[nums[i-k]] == 0 {
			v--
		}
		ans = append(ans, v)
	}
	return
}
```

#### TypeScript

```ts
function distinctNumbers(nums: number[], k: number): number[] {
    const m = Math.max(...nums);
    const cnt: number[] = Array(m + 1).fill(0);
    let v: number = 0;
    for (let i = 0; i < k; ++i) {
        if (++cnt[nums[i]] === 1) {
            v++;
        }
    }
    const ans: number[] = [v];
    for (let i = k; i < nums.length; ++i) {
        if (++cnt[nums[i]] === 1) {
            v++;
        }
        if (--cnt[nums[i - k]] === 0) {
            v--;
        }
        ans.push(v);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
