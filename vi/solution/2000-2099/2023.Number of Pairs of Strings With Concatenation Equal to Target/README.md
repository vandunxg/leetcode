---
comments: true
difficulty: Medium
rating: 1341
source: Biweekly Contest 62 Q2
tags:
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2023. Number of Pairs of Strings With Concatenation Equal to Target](https://leetcode.com/problems/number-of-pairs-of-strings-with-concatenation-equal-to-target)

[中文文档](/solution/2000-2099/2023.Number%20of%20Pairs%20of%20Strings%20With%20Concatenation%20Equal%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các chuỗi <strong>chữ số</strong> <code>nums</code> và một chuỗi <strong>chữ số</strong> <code>target</code>, hãy trả về <em>số cặp chỉ số </em><code>(i, j)</code><em> (trong đó </em><code>i != j</code><em>) sao cho phép <strong>nối chuỗi</strong> </em><code>nums[i] + nums[j]</code><em> bằng </em><code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;777&quot;,&quot;7&quot;,&quot;77&quot;,&quot;77&quot;], target = &quot;7777&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các cặp hợp lệ là:
- (0, 1): &quot;777&quot; + &quot;7&quot;
- (1, 0): &quot;7&quot; + &quot;777&quot;
- (2, 3): &quot;77&quot; + &quot;77&quot;
- (3, 2): &quot;77&quot; + &quot;77&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;123&quot;,&quot;4&quot;,&quot;12&quot;,&quot;34&quot;], target = &quot;1234&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các cặp hợp lệ là:
- (0, 1): &quot;123&quot; + &quot;4&quot;
- (2, 3): &quot;12&quot; + &quot;34&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;1&quot;,&quot;1&quot;,&quot;1&quot;], target = &quot;11&quot;
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các cặp hợp lệ là:
- (0, 1): &quot;1&quot; + &quot;1&quot;
- (1, 0): &quot;1&quot; + &quot;1&quot;
- (0, 2): &quot;1&quot; + &quot;1&quot;
- (2, 0): &quot;1&quot; + &quot;1&quot;
- (1, 2): &quot;1&quot; + &quot;1&quot;
- (2, 1): &quot;1&quot; + &quot;1&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i].length &lt;= 100</code></li>
	<li><code>2 &lt;= target.length &lt;= 100</code></li>
	<li><code>nums[i]</code> và <code>target</code> chỉ gồm các chữ số.</li>
	<li><code>nums[i]</code> và <code>target</code> không có số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 100$ và các chuỗi ngắn, ta có thể liệt kê các cặp có thứ tự $(i,j)$ rồi nối chúng. Điều kiện $i \neq j$ nghĩa là chỉ được dùng lại một giá trị khi giá trị đó xuất hiện ít nhất hai lần.
>
> Hai vòng lặp lồng nhau bám sát đề bài; không cần tiền xử lý.

<!-- thinking:end -->

Duyệt mảng `nums`, với mỗi $i$, ta liệt kê mọi $j$. Nếu $i \neq j$ và $nums[i] + nums[j] = target$, ta tăng đáp án lên một đơn vị.

Độ phức tạp thời gian là $O(n^2 \times m)$, trong đó $n$ và $m$ lần lượt là độ dài của mảng `nums` và chuỗi `target`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfPairs(self, nums: List[str], target: str) -> int:
        n = len(nums)
        return sum(
            i != j and nums[i] + nums[j] == target for i in range(n) for j in range(n)
        )
```

#### Java

```java
class Solution {
    public int numOfPairs(String[] nums, String target) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j && target.equals(nums[i] + nums[j])) {
                    ++ans;
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
    int numOfPairs(vector<string>& nums, string target) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j && nums[i] + nums[j] == target) ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numOfPairs(nums []string, target string) (ans int) {
	for i, a := range nums {
		for j, b := range nums {
			if i != j && a+b == target {
				ans++
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 thực hiện phép nối trong $O(m)$ cho mỗi cặp. Mỗi cặp hợp lệ đều gồm một tiền tố của $target$ và hậu tố tương ứng.
>
> Ta đếm tần suất các chuỗi, sau đó chia $target$ tại mọi vị trí. Khi tiền tố trùng với hậu tố, dùng $c(c-1)$ để không ghép một chỉ số với chính nó.

<!-- thinking:end -->

Ta có thể dùng bảng băm để đếm số lần xuất hiện của mỗi chuỗi trong mảng `nums`, sau đó duyệt tất cả tiền tố và hậu tố của chuỗi `target`. Nếu cả tiền tố và hậu tố đều có trong bảng băm, ta tăng đáp án thêm tích số lần xuất hiện của chúng.

Độ phức tạp thời gian là $O(n + m^2)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng `nums` và chuỗi `target`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfPairs(self, nums: List[str], target: str) -> int:
        cnt = Counter(nums)
        ans = 0
        for i in range(1, len(target)):
            a, b = target[:i], target[i:]
            if a != b:
                ans += cnt[a] * cnt[b]
            else:
                ans += cnt[a] * (cnt[a] - 1)
        return ans
```

#### Java

```java
class Solution {
    public int numOfPairs(String[] nums, String target) {
        Map<String, Integer> cnt = new HashMap<>();
        for (String x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        int ans = 0;
        for (int i = 1; i < target.length(); ++i) {
            String a = target.substring(0, i);
            String b = target.substring(i);
            int x = cnt.getOrDefault(a, 0);
            int y = cnt.getOrDefault(b, 0);
            if (!a.equals(b)) {
                ans += x * y;
            } else {
                ans += x * (y - 1);
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
    int numOfPairs(vector<string>& nums, string target) {
        unordered_map<string, int> cnt;
        for (auto& x : nums) ++cnt[x];
        int ans = 0;
        for (int i = 1; i < target.size(); ++i) {
            string a = target.substr(0, i);
            string b = target.substr(i);
            int x = cnt[a], y = cnt[b];
            if (a != b) {
                ans += x * y;
            } else {
                ans += x * (y - 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numOfPairs(nums []string, target string) (ans int) {
	cnt := map[string]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for i := 1; i < len(target); i++ {
		a, b := target[:i], target[i:]
		if a != b {
			ans += cnt[a] * cnt[b]
		} else {
			ans += cnt[a] * (cnt[a] - 1)
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
