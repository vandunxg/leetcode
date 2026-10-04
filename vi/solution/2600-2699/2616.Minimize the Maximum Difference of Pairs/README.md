---
comments: true
difficulty: Medium
rating: 2155
source: Weekly Contest 340 Q3
tags:
    - Greedy
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [2616. Minimize the Maximum Difference of Pairs](https://leetcode.com/problems/minimize-the-maximum-difference-of-pairs)

[中文文档](/solution/2600-2699/2616.Minimize%20the%20Maximum%20Difference%20of%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>p</code>. Hãy tìm <code>p</code> cặp chỉ số trong <code>nums</code> sao cho <strong>hiệu lớn nhất</strong> giữa các cặp là <strong>nhỏ nhất</strong>. Đồng thời, bảo đảm không có chỉ số nào xuất hiện quá một lần trong <code>p</code> cặp.</p>

<p>Lưu ý rằng với một cặp phần tử tại các chỉ số <code>i</code> và <code>j</code>, hiệu của cặp này là <code>|nums[i] - nums[j]|</code>, trong đó <code>|x|</code> biểu thị <strong>giá trị</strong> <strong>tuyệt đối</strong> của <code>x</code>.</p>

<p>Trả về <em><strong>giá trị nhỏ nhất</strong> của <strong>hiệu lớn nhất</strong> trong số tất cả </em><code>p</code> <em>cặp.</em> Quy ước giá trị lớn nhất của một tập rỗng là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,1,2,7,1,3], p = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Cặp thứ nhất gồm các chỉ số 1 và 4, cặp thứ hai gồm các chỉ số 2 và 5.
Hiệu lớn nhất là max(|nums[1] - nums[4]|, |nums[2] - nums[5]|) = max(0, 1) = 1. Vì vậy, ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,1,2], p = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chọn các chỉ số 1 và 3 để tạo thành một cặp. Hiệu của cặp này là |2 - 2| = 0, đây là giá trị nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= p &lt;= (nums.length)/2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần chọn $p$ cặp rời nhau và tối thiểu hóa hiệu lớn nhất của một cặp. Việc tìm các matching mang tính tổ hợp; với $n \le 10^5$, ta cần một phương pháp đa thức.
>
> Ngưỡng $x$ có tính đơn điệu: nếu tồn tại $p$ cặp với hiệu không vượt quá $x$, thì một $x$ lớn hơn cũng thỏa mãn. Sau khi sắp xếp, các phần tử kề nhau tạo thành những cặp rẻ nhất; chọn ngay một cặp khả thi sẽ để lại các chỉ số phía sau cho những cặp khác.
>
> Ta tìm kiếm nhị phân $x$ và thực hiện bước kiểm tra tham lam đó.

<!-- thinking:end -->

Ta nhận thấy hiệu lớn nhất có tính đơn điệu: nếu một hiệu lớn nhất $x$ là khả thi, thì $x-1$ cũng khả thi. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm hiệu lớn nhất nhỏ nhất có thể đạt được.

Đầu tiên, sắp xếp mảng $\textit{nums}$. Sau đó, với một hiệu lớn nhất $x$ cho trước, kiểm tra xem có thể tạo ra $p$ cặp chỉ số sao cho hiệu lớn nhất của mỗi cặp không vượt quá $x$ hay không. Nếu có thể, ta thử một $x$ nhỏ hơn; nếu không, ta cần tăng $x$.

Để kiểm tra xem có tồn tại $p$ cặp với hiệu lớn nhất không vượt quá $x$ hay không, ta dùng phương pháp tham lam. Duyệt mảng $\textit{nums}$ đã sắp xếp từ trái sang phải. Với chỉ số hiện tại $i$, nếu hiệu giữa $\textit{nums}[i+1]$ và $\textit{nums}[i]$ không vượt quá $x$, ta có thể ghép $i$ và $i+1$ thành một cặp, tăng số cặp $cnt$ lên 1 và tăng $i$ thêm $2$. Nếu không, tăng $i$ thêm $1$. Sau khi duyệt xong, nếu $cnt \geq p$ thì tồn tại $p$ cặp như vậy; ngược lại thì không.

Độ phức tạp thời gian là $O(n \times (\log n + \log m))$, trong đó $n$ là độ dài của $\textit{nums}$ và $m$ là hiệu giữa giá trị lớn nhất và nhỏ nhất trong $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimizeMax(self, nums: List[int], p: int) -> int:
        def check(diff: int) -> bool:
            cnt = i = 0
            while i < len(nums) - 1:
                if nums[i + 1] - nums[i] <= diff:
                    cnt += 1
                    i += 2
                else:
                    i += 1
            return cnt >= p

        nums.sort()
        return bisect_left(range(nums[-1] - nums[0] + 1), True, key=check)
```

#### Java

```java
class Solution {
    public int minimizeMax(int[] nums, int p) {
        Arrays.sort(nums);
        int n = nums.length;
        int l = 0, r = nums[n - 1] - nums[0] + 1;
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (count(nums, mid) >= p) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private int count(int[] nums, int diff) {
        int cnt = 0;
        for (int i = 0; i < nums.length - 1; ++i) {
            if (nums[i + 1] - nums[i] <= diff) {
                ++cnt;
                ++i;
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimizeMax(vector<int>& nums, int p) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int l = 0, r = nums[n - 1] - nums[0] + 1;
        auto check = [&](int diff) -> bool {
            int cnt = 0;
            for (int i = 0; i < n - 1; ++i) {
                if (nums[i + 1] - nums[i] <= diff) {
                    ++cnt;
                    ++i;
                }
            }
            return cnt >= p;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minimizeMax(nums []int, p int) int {
	sort.Ints(nums)
	n := len(nums)
	r := nums[n-1] - nums[0] + 1
	return sort.Search(r, func(diff int) bool {
		cnt := 0
		for i := 0; i < n-1; i++ {
			if nums[i+1]-nums[i] <= diff {
				cnt++
				i++
			}
		}
		return cnt >= p
	})
}
```

#### TypeScript

```ts
function minimizeMax(nums: number[], p: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let l = 0,
        r = nums[n - 1] - nums[0] + 1;
    const check = (diff: number): boolean => {
        let cnt = 0;
        for (let i = 0; i < n - 1; ++i) {
            if (nums[i + 1] - nums[i] <= diff) {
                ++cnt;
                ++i;
            }
        }
        return cnt >= p;
    };
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimize_max(mut nums: Vec<i32>, p: i32) -> i32 {
        nums.sort();
        let n = nums.len();
        let (mut l, mut r) = (0, nums[n - 1] - nums[0] + 1);

        let check = |diff: i32| -> bool {
            let mut cnt = 0;
            let mut i = 0;
            while i < n - 1 {
                if nums[i + 1] - nums[i] <= diff {
                    cnt += 1;
                    i += 2;
                } else {
                    i += 1;
                }
            }
            cnt >= p
        };

        while l < r {
            let mid = (l + r) / 2;
            if check(mid) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        l
    }
}
```

#### C#

```cs
public class Solution {
    public int MinimizeMax(int[] nums, int p) {
        Array.Sort(nums);
        int n = nums.Length;
        int l = 0, r = nums[n - 1] - nums[0] + 1;

        bool check(int diff) {
            int cnt = 0;
            for (int i = 0; i < n - 1; ++i) {
                if (nums[i + 1] - nums[i] <= diff) {
                    ++cnt;
                    ++i;
                }
            }
            return cnt >= p;
        }

        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        return l;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @param Integer $p
     * @return Integer
     */
    function minimizeMax($nums, $p) {
        sort($nums);
        $n = count($nums);
        $l = 0;
        $r = $nums[$n - 1] - $nums[0] + 1;

        $check = function ($diff) use ($nums, $n, $p) {
            $cnt = 0;
            for ($i = 0; $i < $n - 1; ++$i) {
                if ($nums[$i + 1] - $nums[$i] <= $diff) {
                    ++$cnt;
                    ++$i;
                }
            }
            return $cnt >= $p;
        };

        while ($l < $r) {
            $mid = intdiv($l + $r, 2);
            if ($check($mid)) {
                $r = $mid;
            } else {
                $l = $mid + 1;
            }
        }

        return $l;
    }
}
```

#### Swift

```swift
class Solution {
    func minimizeMax(_ nums: [Int], _ p: Int) -> Int {
        var nums = nums.sorted()
        let n = nums.count
        var l = 0
        var r = nums[n - 1] - nums[0] + 1

        func check(_ diff: Int) -> Bool {
            var cnt = 0
            var i = 0
            while i < n - 1 {
                if nums[i + 1] - nums[i] <= diff {
                    cnt += 1
                    i += 2
                } else {
                    i += 1
                }
            }
            return cnt >= p
        }

        while l < r {
            let mid = (l + r) >> 1
            if check(mid) {
                r = mid
            } else {
                l = mid + 1
            }
        }

        return l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
