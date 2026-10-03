---
comments: true
difficulty: Easy
rating: 1228
source: Biweekly Contest 45 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1748. Sum of Unique Elements](https://leetcode.com/problems/sum-of-unique-elements)

[中文文档](/solution/1700-1799/1748.Sum%20of%20Unique%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Các phần tử duy nhất của một mảng là những phần tử xuất hiện <strong>đúng một lần</strong> trong mảng.</p>

<p>Trả về <em><strong>tổng</strong> của tất cả phần tử duy nhất trong </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các phần tử duy nhất là [1,3], và tổng là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có phần tử duy nhất nào, nên tổng là 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Các phần tử duy nhất là [1,2,3,4,5], và tổng là 15.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cộng các giá trị xuất hiện đúng một lần. Cả độ dài và giá trị đều không quá $100$, nên ta đếm tần suất rồi cộng các khóa có số lần xuất hiện bằng $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfUnique(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        return sum(x for x, v in cnt.items() if v == 1)
```

#### Java

```java
class Solution {
    public int sumOfUnique(int[] nums) {
        int[] cnt = new int[101];
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0;
        for (int x = 0; x < 101; ++x) {
            if (cnt[x] == 1) {
                ans += x;
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
    int sumOfUnique(vector<int>& nums) {
        int cnt[101]{};
        for (int& x : nums) {
            ++cnt[x];
        }
        int ans = 0;
        for (int x = 0; x < 101; ++x) {
            if (cnt[x] == 1) {
                ans += x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfUnique(nums []int) (ans int) {
	cnt := [101]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for x := 0; x < 101; x++ {
		if cnt[x] == 1 {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function sumOfUnique(nums: number[]): number {
    const cnt = new Array(101).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    let ans = 0;
    for (let x = 0; x < 101; ++x) {
        if (cnt[x] == 1) {
            ans += x;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_unique(nums: Vec<i32>) -> i32 {
        let mut cnt = [0; 101];
        for x in nums {
            cnt[x as usize] += 1;
        }
        let mut ans = 0;
        for x in 1..101 {
            if cnt[x] == 1 {
                ans += x;
            }
        }
        ans as i32
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function sumOfUnique($nums) {
        $sum = 0;
        for ($i = 0; $i < count($nums); $i++) {
            $hashtable[$nums[$i]] += 1;
            if ($hashtable[$nums[$i]] == 1) {
                $sum += $nums[$i];
            }
            if ($hashtable[$nums[$i]] == 2) {
                $sum -= $nums[$i];
            }
        }
        return $sum;
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
> Lời giải 1 đếm đầy đủ rồi mới tính tổng. Với miền giá trị nhỏ, ta có thể làm trong một lượt: cộng ở lần gặp đầu tiên, trừ ở lần thứ hai và bỏ qua các lần sau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int sumOfUnique(int[] nums) {
        int ans = 0;
        int[] cnt = new int[101];
        for (int x : nums) {
            if (++cnt[x] == 1) {
                ans += x;
            } else if (cnt[x] == 2) {
                ans -= x;
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
    int sumOfUnique(vector<int>& nums) {
        int ans = 0;
        int cnt[101]{};
        for (int& x : nums) {
            if (++cnt[x] == 1) {
                ans += x;
            } else if (cnt[x] == 2) {
                ans -= x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfUnique(nums []int) (ans int) {
	cnt := [101]int{}
	for _, x := range nums {
		cnt[x]++
		if cnt[x] == 1 {
			ans += x
		} else if cnt[x] == 2 {
			ans -= x
		}
	}
	return
}
```

#### TypeScript

```ts
function sumOfUnique(nums: number[]): number {
    let ans = 0;
    const cnt = new Array(101).fill(0);
    for (const x of nums) {
        if (++cnt[x] === 1) {
            ans += x;
        } else if (cnt[x] === 2) {
            ans -= x;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn sum_of_unique(nums: Vec<i32>) -> i32 {
        let mut res = 0;
        let mut map = HashMap::new();
        for num in nums {
            if map.contains_key(&num) {
                if *map.get(&num).unwrap() {
                    map.insert(num, false);
                    res -= num;
                }
            } else {
                map.insert(num, true);
                res += num;
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
