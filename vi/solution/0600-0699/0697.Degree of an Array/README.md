---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [697. Degree of an Array](https://leetcode.com/problems/degree-of-an-array)

[中文文档](/solution/0600-0699/0697.Degree%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng không rỗng gồm các số nguyên không âm <code>nums</code>. <b>Bậc</b> của mảng được định nghĩa là tần suất lớn nhất của một phần tử bất kỳ.</p>

<p>Nhiệm vụ của bạn là tìm độ dài nhỏ nhất có thể của một mảng con liên tiếp trong <code>nums</code> có bậc bằng bậc của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,3,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 
Mảng đầu vào có bậc bằng 2 vì cả hai phần tử 1 và 2 đều xuất hiện hai lần.
Trong các mảng con có cùng bậc:
[1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
Độ dài ngắn nhất là 2. Vì vậy, trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,3,1,4,2]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 
Bậc của mảng là 3 vì phần tử 2 xuất hiện 3 lần.
Vì vậy, [2,2,3,1,4,2] là mảng con ngắn nhất, nên kết quả trả về là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length</code> nằm trong khoảng từ 1 đến 50,000.</li>
	<li><code>nums[i]</code> là số nguyên trong khoảng từ 0 đến 49,999.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bậc là tần suất lớn nhất. Mảng con ngắn nhất có cùng bậc phải chứa lần xuất hiện đầu tiên và cuối cùng của một phần tử có tần suất lớn nhất.
>
> Đếm tần suất và lưu vị trí đầu, cuối. Với các giá trị đạt bậc của mảng, lấy giá trị nhỏ nhất của $right-left+1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findShortestSubArray(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        degree = cnt.most_common()[0][1]
        left, right = {}, {}
        for i, v in enumerate(nums):
            if v not in left:
                left[v] = i
            right[v] = i
        ans = inf
        for v in nums:
            if cnt[v] == degree:
                t = right[v] - left[v] + 1
                if ans > t:
                    ans = t
        return ans
```

#### Java

```java
class Solution {
    public int findShortestSubArray(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        Map<Integer, Integer> left = new HashMap<>();
        Map<Integer, Integer> right = new HashMap<>();
        int degree = 0;
        for (int i = 0; i < nums.length; ++i) {
            int v = nums[i];
            cnt.put(v, cnt.getOrDefault(v, 0) + 1);
            degree = Math.max(degree, cnt.get(v));
            if (!left.containsKey(v)) {
                left.put(v, i);
            }
            right.put(v, i);
        }
        int ans = 1000000;
        for (int v : nums) {
            if (cnt.get(v) == degree) {
                int t = right.get(v) - left.get(v) + 1;
                if (ans > t) {
                    ans = t;
                }
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
    int findShortestSubArray(vector<int>& nums) {
        unordered_map<int, int> cnt;
        unordered_map<int, int> left;
        unordered_map<int, int> right;
        int degree = 0;
        for (int i = 0; i < nums.size(); ++i) {
            int v = nums[i];
            degree = max(degree, ++cnt[v]);
            if (!left.count(v)) {
                left[v] = i;
            }
            right[v] = i;
        }
        int ans = 1e6;
        for (int v : nums) {
            if (cnt[v] == degree) {
                int t = right[v] - left[v] + 1;
                if (ans > t) {
                    ans = t;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findShortestSubArray(nums []int) int {
	cnt := map[int]int{}
	left := map[int]int{}
	right := map[int]int{}
	var degree int
	for i, v := range nums {
		cnt[v]++
		if degree < cnt[v] {
			degree = cnt[v]
		}
		if _, ok := left[v]; !ok {
			left[v] = i
		}
		right[v] = i
	}
	ans := 100000
	for v, c := range cnt {
		if c == degree {
			t := right[v] - left[v] + 1
			if ans > t {
				ans = t
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 ghi nhận các vị trí đầu cuối trong một lượt duyệt. Ta cũng có thể tìm các giá trị có tần suất lớn nhất trước, rồi quét để tìm chỉ số đầu và cuối của từng giá trị đó. Cách này cần thêm các hàm hỗ trợ, nhưng độ phức tạp không đổi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Go

```go
func findShortestSubArray(nums []int) (ans int) {
	ans = 50000
	numsMap := make(map[int]int, len(nums))
	for _, num := range nums {
		numsMap[num]++
	}
	var maxDegree int
	for _, num := range numsMap {
		maxDegree = max(num, maxDegree)
	}
	degreeNums := getMaxDegreeElem(maxDegree, numsMap)
	for _, num := range degreeNums {
		f := findSubArray(num, nums)
		ans = min(ans, f)
	}
	return
}

func findSubArray(target int, nums []int) int {
	start := getStartIdx(target, nums)
	end := getEndIdx(target, nums)
	return (end - start) + 1
}

func getStartIdx(target int, nums []int) (start int) {
	for idx, num := range nums {
		if num == target {
			start = idx
			break
		}
	}
	return start
}

func getEndIdx(target int, nums []int) (end int) {
	for i := len(nums) - 1; i > 0; i-- {
		if nums[i] == target {
			end = i
			break
		}
	}
	return
}

func getMaxDegreeElem(maxDegree int, numsMap map[int]int) []int {
	var ans []int
	for key, value := range numsMap {
		if value == maxDegree {
			ans = append(ans, key)
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
