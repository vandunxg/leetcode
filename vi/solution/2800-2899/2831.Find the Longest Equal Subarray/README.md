---
comments: true
difficulty: Medium
rating: 1975
source: Weekly Contest 359 Q4
tags:
    - Array
    - Hash Table
    - Binary Search
    - Sliding Window
---

<!-- problem:start -->

# [2831. Find the Longest Equal Subarray](https://leetcode.com/problems/find-the-longest-equal-subarray)

[中文文档](/solution/2800-2899/2831.Find%20the%20Longest%20Equal%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một mảng con được gọi là <strong>đồng nhất</strong> nếu tất cả phần tử của nó đều bằng nhau. Lưu ý rằng mảng con rỗng cũng là một mảng con <strong>đồng nhất</strong>.</p>

<p>Hãy trả về <em>độ dài của mảng con đồng nhất <strong>dài nhất</strong> có thể sau khi xóa <strong>nhiều nhất</strong> </em><code>k</code><em> phần tử khỏi </em><code>nums</code>.</p>

<p>Một <b>mảng con</b> là một dãy phần tử liên tiếp, có thể rỗng, nằm trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,3,1,3], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Xóa các phần tử tại chỉ số 2 và chỉ số 4 là tối ưu.
Sau khi xóa, nums trở thành [1, 3, 3, 3].
Mảng con đồng nhất dài nhất bắt đầu tại i = 1 và kết thúc tại j = 3, có độ dài bằng 3.
Có thể chứng minh rằng không thể tạo ra mảng con đồng nhất dài hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,2,1,1], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Xóa các phần tử tại chỉ số 2 và chỉ số 3 là tối ưu.
Sau khi xóa, nums trở thành [1, 1, 1, 1].
Bản thân mảng là một mảng con đồng nhất, nên đáp án là 4.
Có thể chứng minh rằng không thể tạo ra mảng con đồng nhất dài hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
	<li><code>0 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con đồng nhất có thể xóa nhiều nhất $k$ giá trị khác. Một cửa sổ không hợp lệ khi số phần tử không thuộc nhóm chiếm đa số vượt quá $k$. Hash map lưu số lần xuất hiện lớn nhất $mx$; thu hẹp từ bên trái khi $r-l+1-mx>k$. Đáp án chính là số lần xuất hiện lớn nhất đó.

<!-- thinking:end -->

Ta dùng hai con trỏ để duy trì một cửa sổ có độ dài thay đổi theo một chiều, đồng thời dùng hash table để duy trì số lần xuất hiện của mỗi phần tử trong cửa sổ.

Số lượng tất cả phần tử trong cửa sổ trừ đi số lượng phần tử xuất hiện nhiều nhất trong cửa sổ chính là số phần tử cần xóa khỏi cửa sổ.

Mỗi lần, ta thêm phần tử mà con trỏ phải đang trỏ tới vào cửa sổ, sau đó cập nhật hash table và số lượng phần tử xuất hiện nhiều nhất trong cửa sổ. Khi số phần tử cần xóa vượt quá $k$, ta dịch con trỏ trái một lần rồi cập nhật hash table.

Sau khi duyệt xong, ta trả về số lần xuất hiện của phần tử xuất hiện nhiều nhất.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestEqualSubarray(self, nums: List[int], k: int) -> int:
        cnt = Counter()
        l = 0
        mx = 0
        for r, x in enumerate(nums):
            cnt[x] += 1
            mx = max(mx, cnt[x])
            if r - l + 1 - mx > k:
                cnt[nums[l]] -= 1
                l += 1
        return mx
```

#### Java

```java
class Solution {
    public int longestEqualSubarray(List<Integer> nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int mx = 0, l = 0;
        for (int r = 0; r < nums.size(); ++r) {
            cnt.merge(nums.get(r), 1, Integer::sum);
            mx = Math.max(mx, cnt.get(nums.get(r)));
            if (r - l + 1 - mx > k) {
                cnt.merge(nums.get(l++), -1, Integer::sum);
            }
        }
        return mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestEqualSubarray(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        int mx = 0, l = 0;
        for (int r = 0; r < nums.size(); ++r) {
            mx = max(mx, ++cnt[nums[r]]);
            if (r - l + 1 - mx > k) {
                --cnt[nums[l++]];
            }
        }
        return mx;
    }
};
```

#### Go

```go
func longestEqualSubarray(nums []int, k int) int {
	cnt := map[int]int{}
	mx, l := 0, 0
	for r, x := range nums {
		cnt[x]++
		mx = max(mx, cnt[x])
		if r-l+1-mx > k {
			cnt[nums[l]]--
			l++
		}
	}
	return mx
}
```

#### TypeScript

```ts
function longestEqualSubarray(nums: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    let mx = 0;
    let l = 0;
    for (let r = 0; r < nums.length; ++r) {
        cnt.set(nums[r], (cnt.get(nums[r]) ?? 0) + 1);
        mx = Math.max(mx, cnt.get(nums[r])!);
        if (r - l + 1 - mx > k) {
            cnt.set(nums[l], cnt.get(nums[l])! - 1);
            ++l;
        }
    }
    return mx;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- source:start -->

### Lời giải 2: Hash Table + Hai con trỏ (Phương pháp 2)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 duy trì số lần xuất hiện của mọi giá trị trong một cửa sổ. Việc nhóm các chỉ số theo giá trị cho phép mỗi lần quét bằng hai con trỏ chỉ đếm các khoảng cách giữa những lần xuất hiện của giá trị đó, với cùng giới hạn tuyến tính.

<!-- thinking:end -->

Ta có thể dùng hash table $g$ để lưu danh sách chỉ số của mỗi phần tử.

Tiếp theo, ta lần lượt chọn mỗi phần tử làm giá trị cần đồng nhất. Ta lấy danh sách chỉ số $ids$ của phần tử này từ hash table $g$. Sau đó, ta dùng hai con trỏ $l$ và $r$ để duy trì một cửa sổ sao cho số phần tử trong cửa sổ trừ đi số phần tử có giá trị cần đồng nhất không vượt quá $k$. Vì vậy, ta chỉ cần tìm cửa sổ lớn nhất thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestEqualSubarray(self, nums: List[int], k: int) -> int:
        g = defaultdict(list)
        for i, x in enumerate(nums):
            g[x].append(i)
        ans = 0
        for ids in g.values():
            l = 0
            for r in range(len(ids)):
                while ids[r] - ids[l] - (r - l) > k:
                    l += 1
                ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestEqualSubarray(List<Integer> nums, int k) {
        int n = nums.size();
        List<Integer>[] g = new List[n + 1];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            g[nums.get(i)].add(i);
        }
        int ans = 0;
        for (List<Integer> ids : g) {
            int l = 0;
            for (int r = 0; r < ids.size(); ++r) {
                while (ids.get(r) - ids.get(l) - (r - l) > k) {
                    ++l;
                }
                ans = Math.max(ans, r - l + 1);
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
    int longestEqualSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> g[n + 1];
        for (int i = 0; i < n; ++i) {
            g[nums[i]].push_back(i);
        }
        int ans = 0;
        for (const auto& ids : g) {
            int l = 0;
            for (int r = 0; r < ids.size(); ++r) {
                while (ids[r] - ids[l] - (r - l) > k) {
                    ++l;
                }
                ans = max(ans, r - l + 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestEqualSubarray(nums []int, k int) (ans int) {
	g := make([][]int, len(nums)+1)
	for i, x := range nums {
		g[x] = append(g[x], i)
	}
	for _, ids := range g {
		l := 0
		for r := range ids {
			for ids[r]-ids[l]-(r-l) > k {
				l++
			}
			ans = max(ans, r-l+1)
		}
	}
	return
}
```

#### TypeScript

```ts
function longestEqualSubarray(nums: number[], k: number): number {
    const n = nums.length;
    const g: number[][] = Array.from({ length: n + 1 }, () => []);
    for (let i = 0; i < n; ++i) {
        g[nums[i]].push(i);
    }
    let ans = 0;
    for (const ids of g) {
        let l = 0;
        for (let r = 0; r < ids.length; ++r) {
            while (ids[r] - ids[l] - (r - l) > k) {
                ++l;
            }
            ans = Math.max(ans, r - l + 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
