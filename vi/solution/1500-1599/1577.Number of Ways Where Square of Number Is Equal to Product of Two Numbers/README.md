---
comments: true
difficulty: Medium
rating: 1593
source: Weekly Contest 205 Q2
tags:
    - Array
    - Hash Table
    - Math
    - Two Pointers
---

<!-- problem:start -->

# [1577. Number of Ways Where Square of Number Is Equal to Product of Two Numbers](https://leetcode.com/problems/number-of-ways-where-square-of-number-is-equal-to-product-of-two-numbers)

[中文文档](/solution/1500-1599/1577.Number%20of%20Ways%20Where%20Square%20of%20Number%20Is%20Equal%20to%20Product%20of%20Two%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, hãy trả về số bộ ba (loại 1 và loại 2) được tạo theo các quy tắc sau:</p>

<ul>
	<li>Loại 1: Bộ ba (i, j, k) nếu <code>nums1[i]<sup>2</sup> == nums2[j] * nums2[k]</code>, với <code>0 &lt;= i &lt; nums1.length</code> và <code>0 &lt;= j &lt; k &lt; nums2.length</code>.</li>
	<li>Loại 2: Bộ ba (i, j, k) nếu <code>nums2[i]<sup>2</sup> == nums1[j] * nums1[k]</code>, với <code>0 &lt;= i &lt; nums2.length</code> và <code>0 &lt;= j &lt; k &lt; nums1.length</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [7,4], nums2 = [5,2,8,9]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Loại 1: (1, 1, 2), nums1[1]<sup>2</sup> = nums2[1] * nums2[2]. (4<sup>2</sup> = 2 * 8). 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,1], nums2 = [1,1,1]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Mọi bộ ba đều hợp lệ vì 1<sup>2</sup> = 1 * 1.
Loại 1: (0,0,1), (0,0,2), (0,1,2), (1,0,1), (1,0,2), (1,1,2).  nums1[i]<sup>2</sup> = nums2[j] * nums2[k].
Loại 2: (0,0,1), (1,0,1), (2,0,1). nums2[i]<sup>2</sup> = nums1[j] * nums1[k].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [7,7,8,3], nums2 = [1,2,9,7]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 bộ ba hợp lệ.
Loại 1: (3,0,2).  nums1[3]<sup>2</sup> = nums2[0] * nums2[2].
Loại 2: (3,0,1).  nums2[3]<sup>2</sup> = nums1[0] * nums1[1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba thỏa $nums1[i]^2=nums2[j]\cdot nums2[k]$ và trường hợp đối xứng. Độ dài thường khoảng $100$, nên ba vòng lặp vẫn đủ nhanh, nhưng cùng một tích có thể được truy vấn nhiều lần.
>
> Dùng hash để đếm mọi tích của cặp không có thứ tự ở một phía, rồi với mỗi $x$ ở phía kia truy vấn $x^2$. Đổi vai trò hai mảng và cộng các số đếm.

<!-- thinking:end -->

Ta dùng hash table $\textit{cnt1}$ để đếm số lần xuất hiện của mỗi cặp $(\textit{nums}[j], \textit{nums}[k])$ trong $\textit{nums1}$, với $0 \leq j < k < m$, và $m$ là độ dài mảng $\textit{nums1}$. Tương tự, ta dùng $\textit{cnt2}$ để đếm các cặp trong $\textit{nums2}$, với $0 \leq j < k < n$, và $n$ là độ dài mảng $\textit{nums2}$.

Tiếp theo, ta duyệt mỗi số $x$ trong mảng $\textit{nums1}$ và tính $\textit{cnt2}[x^2]$, là số cặp $(\textit{nums}[j], \textit{nums}[k])$ trong $\textit{nums2}$ thỏa $\textit{nums}[j] \times \textit{nums}[k] = x^2$. Tương tự, ta duyệt mỗi số $x$ trong $\textit{nums2}$ và tính $\textit{cnt1}[x^2]$. Cuối cùng, trả về tổng hai kết quả.

Độ phức tạp thời gian là $O(m^2 + n^2 + m + n)$ và độ phức tạp không gian là $O(m^2 + n^2)$. Ở đây, $m$ và $n$ lần lượt là độ dài của các mảng $\textit{nums1}$ và $\textit{nums2}$.

Các cặp $(\textit{nums}[j], \textit{nums}[k])$ được đếm; các ký hiệu $y$, $v1$, $v1$ và $z$ được giữ nguyên theo công thức của bài. Ta dùng $\textit{nums1}$, $\textit{cnt2}$, điều kiện $\textit{nums}[j] \times \textit{nums}[k] = x^2$ và hằng số $2$ đúng như định nghĩa; chỉ số $(j, k)$ xác định một cặp $(\textit{nums}[j], \textit{nums}[k])$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        def count(nums: List[int]) -> Counter:
            cnt = Counter()
            for j in range(len(nums)):
                for k in range(j + 1, len(nums)):
                    cnt[nums[j] * nums[k]] += 1
            return cnt

        def cal(nums: List[int], cnt: Counter) -> int:
            return sum(cnt[x * x] for x in nums)

        cnt1 = count(nums1)
        cnt2 = count(nums2)
        return cal(nums1, cnt2) + cal(nums2, cnt1)
```

#### Java

```java
class Solution {
    public int numTriplets(int[] nums1, int[] nums2) {
        var cnt1 = count(nums1);
        var cnt2 = count(nums2);
        return cal(cnt1, nums2) + cal(cnt2, nums1);
    }

    private Map<Long, Integer> count(int[] nums) {
        Map<Long, Integer> cnt = new HashMap<>();
        int n = nums.length;
        for (int j = 0; j < n; ++j) {
            for (int k = j + 1; k < n; ++k) {
                long x = (long) nums[j] * nums[k];
                cnt.merge(x, 1, Integer::sum);
            }
        }
        return cnt;
    }

    private int cal(Map<Long, Integer> cnt, int[] nums) {
        int ans = 0;
        for (int x : nums) {
            long y = (long) x * x;
            ans += cnt.getOrDefault(y, 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numTriplets(vector<int>& nums1, vector<int>& nums2) {
        auto cnt1 = count(nums1);
        auto cnt2 = count(nums2);
        return cal(cnt1, nums2) + cal(cnt2, nums1);
    }

    unordered_map<long long, int> count(vector<int>& nums) {
        unordered_map<long long, int> cnt;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                cnt[(long long) nums[i] * nums[j]]++;
            }
        }
        return cnt;
    }

    int cal(unordered_map<long long, int>& cnt, vector<int>& nums) {
        int ans = 0;
        for (int x : nums) {
            ans += cnt[(long long) x * x];
        }
        return ans;
    }
};
```

#### Go

```go
func numTriplets(nums1 []int, nums2 []int) int {
	cnt1 := count(nums1)
	cnt2 := count(nums2)
	return cal(cnt1, nums2) + cal(cnt2, nums1)
}

func count(nums []int) map[int]int {
	cnt := map[int]int{}
	for j, x := range nums {
		for _, y := range nums[j+1:] {
			cnt[x*y]++
		}
	}
	return cnt
}

func cal(cnt map[int]int, nums []int) (ans int) {
	for _, x := range nums {
		ans += cnt[x*x]
	}
	return
}
```

#### TypeScript

```ts
function numTriplets(nums1: number[], nums2: number[]): number {
    const cnt1 = count(nums1);
    const cnt2 = count(nums2);
    return cal(cnt1, nums2) + cal(cnt2, nums1);
}

function count(nums: number[]): Map<number, number> {
    const cnt: Map<number, number> = new Map();
    for (let j = 0; j < nums.length; ++j) {
        for (let k = j + 1; k < nums.length; ++k) {
            const x = nums[j] * nums[k];
            cnt.set(x, (cnt.get(x) || 0) + 1);
        }
    }
    return cnt;
}

function cal(cnt: Map<number, number>, nums: number[]): number {
    return nums.reduce((acc, x) => acc + (cnt.get(x * x) || 0), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table + Tối ưu liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu mọi tích của cặp và dùng bộ nhớ $O(n^2)$. Thay vào đó, ta lưu tần suất, duyệt $x$ và một giá trị $y$, đặt $z=x^2/y$ nếu chia hết, cộng $v_y(v_z-[y=z])$, rồi chia hai để loại cặp hoán đổi $(y,z)/(z,y)$. Thời gian giảm còn $O(mn)$ với không gian tuyến tính.

<!-- thinking:end -->

Ta dùng hash table $\textit{cnt1}$ để đếm số lần xuất hiện của mỗi số trong $\textit{nums1}$ và hash table $\textit{cnt2}$ để đếm số lần xuất hiện của mỗi số trong $\textit{nums2}$.

Tiếp theo, ta duyệt mỗi số $x$ trong mảng $\textit{nums1}$, rồi duyệt mỗi cặp $(y, v1)$ trong $\textit{cnt2}$, trong đó $y$ là key và $v1$ là value của $\textit{cnt2}$. Ta tính $z = x^2 / y$. Nếu $y \times z = x^2$ và $y = z$, số cách chọn hai số là $v1 \times (v1 - 1) = v1 \times (v2 - 1)$. Nếu $y \neq z$, số cách là $v1 \times v2$. Cuối cùng, ta cộng mọi số cách rồi chia cho $2$, vì cặp $(j, k)$ và $(k, j)$ là cùng một cách.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m + n)$. Ở đây, $m$ và $n$ lần lượt là độ dài của các mảng $\textit{nums1}$ và $\textit{nums2}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        def cal(nums: List[int], cnt: Counter) -> int:
            ans = 0
            for x in nums:
                for y, v1 in cnt.items():
                    z = x * x // y
                    if y * z == x * x:
                        v2 = cnt[z]
                        ans += v1 * (v2 - int(y == z))
            return ans // 2

        cnt1 = Counter(nums1)
        cnt2 = Counter(nums2)
        return cal(nums1, cnt2) + cal(nums2, cnt1)
```

#### Java

```java
class Solution {
    public int numTriplets(int[] nums1, int[] nums2) {
        var cnt1 = count(nums1);
        var cnt2 = count(nums2);
        return cal(cnt1, nums2) + cal(cnt2, nums1);
    }

    private Map<Integer, Integer> count(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        return cnt;
    }

    private int cal(Map<Integer, Integer> cnt, int[] nums) {
        long ans = 0;
        for (int x : nums) {
            for (var e : cnt.entrySet()) {
                int y = e.getKey(), v1 = e.getValue();
                int z = (int) (1L * x * x / y);
                if (y * z == x * x) {
                    int v2 = cnt.getOrDefault(z, 0);
                    ans += v1 * (y == z ? v2 - 1 : v2);
                }
            }
        }
        return (int) (ans / 2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numTriplets(vector<int>& nums1, vector<int>& nums2) {
        auto cnt1 = count(nums1);
        auto cnt2 = count(nums2);
        return cal(cnt1, nums2) + cal(cnt2, nums1);
    }

    unordered_map<int, int> count(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        return cnt;
    }

    int cal(unordered_map<int, int>& cnt, vector<int>& nums) {
        long long ans = 0;
        for (int x : nums) {
            for (auto& [y, v1] : cnt) {
                int z = 1LL * x * x / y;
                if (1LL * y * z == 1LL * x * x) {
                    if (cnt.contains(z)) {
                        int v2 = cnt[z];
                        ans += 1LL * v1 * (y == z ? v2 - 1 : v2);
                    }
                }
            }
        }
        return ans / 2;
    }
};
```

#### Go

```go
func numTriplets(nums1 []int, nums2 []int) int {
	cnt1 := count(nums1)
	cnt2 := count(nums2)
	return cal(cnt1, nums2) + cal(cnt2, nums1)
}

func count(nums []int) map[int]int {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	return cnt
}

func cal(cnt map[int]int, nums []int) (ans int) {
	for _, x := range nums {
		for y, v1 := range cnt {
			z := x * x / y
			if y*z == x*x {
				if v2, ok := cnt[z]; ok {
					if y == z {
						v2--
					}
					ans += v1 * v2
				}
			}
		}
	}
	ans /= 2
	return
}
```

#### TypeScript

```ts
function numTriplets(nums1: number[], nums2: number[]): number {
    const cnt1 = count(nums1);
    const cnt2 = count(nums2);
    return cal(cnt1, nums2) + cal(cnt2, nums1);
}

function count(nums: number[]): Map<number, number> {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    return cnt;
}

function cal(cnt: Map<number, number>, nums: number[]): number {
    let ans: number = 0;
    for (const x of nums) {
        for (const [y, v1] of cnt) {
            const z = Math.floor((x * x) / y);
            if (y * z == x * x) {
                const v2 = cnt.get(z) || 0;
                ans += v1 * (y === z ? v2 - 1 : v2);
            }
        }
    }
    return ans / 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
