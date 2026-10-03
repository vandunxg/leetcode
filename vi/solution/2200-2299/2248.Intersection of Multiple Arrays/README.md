---
comments: true
difficulty: Easy
rating: 1264
source: Weekly Contest 290 Q1
tags:
    - Array
    - Hash Table
    - Counting
    - Sorting
---

<!-- problem:start -->

# [2248. Intersection of Multiple Arrays](https://leetcode.com/problems/intersection-of-multiple-arrays)

[Tài liệu tiếng Trung](/solution/2200-2299/2248.Intersection%20of%20Multiple%20Arrays/README.md)

## Mô tả

<!-- description:start -->

Cho một mảng số nguyên hai chiều <code>nums</code>, trong đó <code>nums[i]</code> là một mảng không rỗng gồm các số nguyên dương <strong>khác nhau</strong>, hãy trả về <em>danh sách các số nguyên xuất hiện trong <strong>mọi mảng</strong> của</em> <code>nums</code><em>, được sắp xếp theo <strong>thứ tự tăng dần</strong></em>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[<u><strong>3</strong></u>,1,2,<u><strong>4</strong></u>,5],[1,2,<u><strong>3</strong></u>,<u><strong>4</strong></u>],[<u><strong>3</strong></u>,<u><strong>4</strong></u>,5,6]]
<strong>Đầu ra:</strong> [3,4]
<strong>Giải thích:</strong>
Các số nguyên duy nhất xuất hiện trong nums[0] = [<u><strong>3</strong></u>,1,2,<u><strong>4</strong></u>,5], nums[1] = [1,2,<u><strong>3</strong></u>,<u><strong>4</strong></u>] và nums[2] = [<u><strong>3</strong></u>,<u><strong>4</strong></u>,5,6] là 3 và 4, nên ta trả về [3,4].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1,2,3],[4,5,6]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>
Không tồn tại số nguyên nào xuất hiện đồng thời trong nums[0] và nums[1], nên ta trả về danh sách rỗng [].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= sum(nums[i].length) &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i][j] &lt;= 1000</code></li>
	<li>Tất cả các giá trị trong <code>nums[i]</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần các giá trị xuất hiện trong mọi mảng con, theo thứ tự tăng dần. Các giá trị trong mỗi mảng con là duy nhất và nằm trong $[1,1000]$. Có thể liên tục lấy giao của các tập hợp, nhưng cách đó sẽ cấp phát bộ nhớ không cần thiết.
>
> Một mảng đếm có độ dài $1001$ sẽ tăng $cnt[x]$ một lần cho mỗi mảng con. Những $x$ có số đếm bằng số lượng mảng con chính là các phần tử thuộc giao, đồng thời đã được sắp xếp theo chỉ số.

<!-- thinking:end -->

Duyệt qua mảng `nums`. Với mỗi mảng con `arr`, ta đếm số lần xuất hiện của từng số trong `arr`. Sau đó, ta duyệt qua mảng đếm và lấy những số xuất hiện số lần bằng độ dài của mảng `nums`; đó chính là đáp án.

Độ phức tạp thời gian là $O(N)$, còn độ phức tạp không gian là $O(1000)$. Trong đó, $N$ là tổng số phần tử trong mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def intersection(self, nums: List[List[int]]) -> List[int]:
        cnt = [0] * 1001
        for arr in nums:
            for x in arr:
                cnt[x] += 1
        return [x for x, v in enumerate(cnt) if v == len(nums)]
```

#### Java

```java
class Solution {
    public List<Integer> intersection(int[][] nums) {
        int[] cnt = new int[1001];
        for (var arr : nums) {
            for (int x : arr) {
                ++cnt[x];
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int x = 0; x < 1001; ++x) {
            if (cnt[x] == nums.length) {
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
    vector<int> intersection(vector<vector<int>>& nums) {
        int cnt[1001]{};
        for (auto& arr : nums) {
            for (int& x : arr) {
                ++cnt[x];
            }
        }
        vector<int> ans;
        for (int x = 0; x < 1001; ++x) {
            if (cnt[x] == nums.size()) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func intersection(nums [][]int) (ans []int) {
	cnt := [1001]int{}
	for _, arr := range nums {
		for _, x := range arr {
			cnt[x]++
		}
	}
	for x, v := range cnt {
		if v == len(nums) {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function intersection(nums: number[][]): number[] {
    const cnt = new Array(1001).fill(0);
    for (const arr of nums) {
        for (const x of arr) {
            cnt[x]++;
        }
    }
    const ans: number[] = [];
    for (let x = 0; x < 1001; x++) {
        if (cnt[x] === nums.length) {
            ans.push(x);
        }
    }
    return ans;
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[][] $nums
     * @return Integer[]
     */
    function intersection($nums) {
        $rs = [];
        for ($i = 0; $i < count($nums); $i++) {
            for ($j = 0; $j < count($nums[$i]); $j++) {
                $hashtable[$nums[$i][$j]] += 1;
                if ($hashtable[$nums[$i][$j]] === count($nums)) {
                    array_push($rs, $nums[$i][$j]);
                }
            }
        }
        sort($rs);
        return $rs;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đợi đến cuối mới thu thập đáp án. Ta có thể thêm $x$ ngay khi $cnt[x]$ đạt đến số lượng mảng con, rồi chỉ cần sắp xếp một lần. Miền giá trị nhỏ nên cả hai phiên bản có cùng bậc độ phức tạp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def intersection(self, nums: List[List[int]]) -> List[int]:
        cnt = Counter()
        ans = []
        for arr in nums:
            for x in arr:
                cnt[x] += 1
                if cnt[x] == len(nums):
                    ans.append(x)
        ans.sort()
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> intersection(int[][] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        List<Integer> ans = new ArrayList<>();
        for (var arr : nums) {
            for (int x : arr) {
                if (cnt.merge(x, 1, Integer::sum) == nums.length) {
                    ans.add(x);
                }
            }
        }
        Collections.sort(ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> intersection(vector<vector<int>>& nums) {
        unordered_map<int, int> cnt;
        vector<int> ans;
        for (auto& arr : nums) {
            for (int& x : arr) {
                if (++cnt[x] == nums.size()) {
                    ans.push_back(x);
                }
            }
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func intersection(nums [][]int) (ans []int) {
	cnt := map[int]int{}
	for _, arr := range nums {
		for _, x := range arr {
			cnt[x]++
			if cnt[x] == len(nums) {
				ans = append(ans, x)
			}
		}
	}
	sort.Ints(ans)
	return
}
```

#### TypeScript

```ts
function intersection(nums: number[][]): number[] {
    const cnt = new Array(1001).fill(0);
    const ans: number[] = [];
    for (const arr of nums) {
        for (const x of arr) {
            if (++cnt[x] == nums.length) {
                ans.push(x);
            }
        }
    }
    ans.sort((a, b) => a - b);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
