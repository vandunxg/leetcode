---
comments: true
difficulty: Hard
rating: 2805
source: Weekly Contest 438 Q4
tags:
    - Geometry
    - Array
    - Math
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3464. Maximize the Distance Between Points on a Square](https://leetcode.com/problems/maximize-the-distance-between-points-on-a-square)

[中文文档](/solution/3400-3499/3464.Maximize%20the%20Distance%20Between%20Points%20on%20a%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code><font face="monospace">side</font></code>, biểu diễn độ dài cạnh của một hình vuông có các đỉnh lần lượt là <code>(0, 0)</code>, <code>(0, side)</code>, <code>(side, 0)</code> và <code>(side, side)</code> trên mặt phẳng Descartes.</p>

<p>Cho một số nguyên <strong>dương</strong> <code>k</code> và một mảng số nguyên 2 chiều <code>points</code>, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> biểu diễn tọa độ của một điểm nằm trên <strong>biên</strong> của hình vuông.</p>

<p>Cần chọn <code>k</code> phần tử trong <code>points</code> sao cho khoảng cách Manhattan <strong>nhỏ nhất</strong> giữa mọi cặp điểm là <strong>lớn nhất</strong>.</p>

<p>Trả về khoảng cách Manhattan <strong>nhỏ nhất</strong> <strong>lớn nhất</strong> có thể đạt được giữa <code>k</code> điểm được chọn.</p>

<p>Khoảng cách Manhattan giữa hai ô <code>(x<sub>i</sub>, y<sub>i</sub>)</code> và <code>(x<sub>j</sub>, y<sub>j</sub>)</code> là <code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">side = 2, points = [[0,2],[2,0],[2,2],[0,0]], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3464.Maximize%20the%20Distance%20Between%20Points%20on%20a%20Square/images/4080_example0_revised.png" style="width: 200px; height: 200px;" /></p>

<p>Chọn cả bốn điểm.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">side = 2, points = [[0,0],[1,2],[2,0],[2,2],[2,1]], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3464.Maximize%20the%20Distance%20Between%20Points%20on%20a%20Square/images/4080_example1_revised.png" style="width: 211px; height: 200px;" /></p>

<p>Chọn các điểm <code>(0, 0)</code>, <code>(2, 0)</code>, <code>(2, 2)</code> và <code>(2, 1)</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">side = 2, points = [[0,0],[0,1],[0,2],[1,2],[2,0],[2,2],[2,1]], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3464.Maximize%20the%20Distance%20Between%20Points%20on%20a%20Square/images/4080_example2_revised.png" style="width: 200px; height: 200px;" /></p>

<p>Chọn các điểm <code>(0, 0)</code>, <code>(0, 1)</code>, <code>(0, 2)</code>, <code>(1, 2)</code> và <code>(2, 2)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= side &lt;= 10<sup>9</sup></code></li>
	<li><code>4 &lt;= points.length &lt;= min(4 * side, 15 * 10<sup>3</sup>)</code></li>
	<li><code>points[i] == [x<sub>i</sub>, y<sub>i</sub>]</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho:
	<ul>
		<li><code>points[i]</code> nằm trên biên của hình vuông.</li>
		<li>Mọi <code>points[i]</code> đều <strong>khác nhau</strong>.</li>
	</ul>
	</li>
	<li><code>4 &lt;= k &lt;= min(25, points.length)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Ánh xạ tọa độ + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn $k$ điểm trên biên hình vuông và tối đa hóa cung nhỏ nhất giữa hai điểm kề nhau, bao gồm cả cung nối vòng về điểm đầu. Cạnh hình vuông rất lớn, có nhiều nhất $1.5\times 10^4$ điểm và $k\le 25$.
>
> Bài toán tối đa hóa một giá trị nhỏ nhất phù hợp với tìm kiếm nhị phân. Trải phẳng biên hình vuông thành một đường tròn có độ dài $4\cdot\textit{side}$ giúp chuyển khoảng cách thành các khoảng cách một chiều.
>
> Với một giá trị ứng viên $\textit{lo}$, ta thử từng điểm bắt đầu và dùng tìm kiếm nhị phân để tham lam nhảy $k-1$ lần, giữ điểm cuối không vượt quá $\textit{start}+4\textit{side}-\textit{lo}$ để vòng tròn khép lại. Nếu khả thi thì tăng $\textit{lo}$.

<!-- thinking:end -->

Vì bài toán yêu cầu tối đa hóa khoảng cách nhỏ nhất, ta có thể dùng tìm kiếm nhị phân trên đáp án để tìm nghiệm tối ưu.

Trước hết, để đơn giản hóa logic, ta ánh xạ tọa độ 2D $(x, y)$ trên biên hình vuông thành một trục 1D $[0, 4 \times \text{side})$. Quy tắc ánh xạ như sau:

- Nếu $x = 0$, giá trị sau khi ánh xạ là $y$;
- Nếu $y = \text{side}$, giá trị sau khi ánh xạ là $\text{side} + x$;
- Nếu $x = \text{side}$, giá trị sau khi ánh xạ là $3 \times \text{side} - y$;
- Nếu không, giá trị sau khi ánh xạ là $4 \times \text{side} - x$.

Sau khi ánh xạ, sắp xếp tất cả các điểm để thu được mảng $\textit{nums}$. Vì các điểm được chọn trên chu vi hình vuông, đây thực chất là một bài toán trên đường tròn.

Trong quá trình tìm kiếm nhị phân, với một khoảng cách nhỏ nhất $\textit{lo}$, ta dùng hàm $\textit{check}$ để kiểm tra tính khả thi:

- Duyệt từng điểm trong $\textit{nums}$ làm điểm bắt đầu $\textit{start}$.
- Điểm cuối để chọn các điểm là $\textit{end} = \textit{start} + 4 \times \text{side} - \textit{lo}$, đảm bảo khoảng cách vòng từ điểm cuối được chọn về $\textit{start}$ ít nhất là $\textit{lo}$.
- Sau đó, thực hiện tham lam $k-1$ lần nhảy, mỗi lần dùng tìm kiếm nhị phân để nhanh chóng tìm điểm tiếp theo cách vị trí hiện tại ít nhất $\textit{lo}$.
- Nếu có thể chọn $k$ điểm trong giới hạn $\textit{end}$ thì khoảng cách $\textit{lo}$ là khả thi.

Độ phức tạp thời gian là $O(n \log (\text{side}) \cdot n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{points}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, side: int, points: List[List[int]], k: int) -> int:
        nums = []
        for x, y in points:
            if x == 0:
                nums.append(y)
            elif y == side:
                nums.append(side + x)
            elif x == side:
                nums.append(side * 3 - y)
            else:
                nums.append(side * 4 - x)
        nums.sort()

        def check(lo: int) -> bool:
            for start in nums:
                end = start + side * 4 - lo
                cur = start
                ok = True
                for _ in range(k - 1):
                    j = bisect_left(nums, cur + lo)
                    if j == len(nums) or nums[j] > end:
                        ok = False
                        break
                    cur = nums[j]
                if ok:
                    return True
            return False

        l, r = 1, side
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private int side;
    private long[] nums;
    private int k;

    public int maxDistance(int side, int[][] points, int k) {
        this.side = side;
        this.k = k;
        int n = points.length;
        this.nums = new long[n];
        for (int i = 0; i < n; i++) {
            int x = points[i][0];
            int y = points[i][1];
            if (x == 0) {
                nums[i] = (long) y;
            } else if (y == side) {
                nums[i] = (long) side + x;
            } else if (x == side) {
                nums[i] = (long) side * 3 - y;
            } else {
                nums[i] = (long) side * 4 - x;
            }
        }
        Arrays.sort(nums);

        int l = 1, r = side;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int lo) {
        long total = (long) side * 4;
        for (int i = 0; i < nums.length; i++) {
            long start = nums[i];
            long end = start + total - lo;
            long cur = start;
            boolean ok = true;
            for (int j = 0; j < k - 1; j++) {
                long target = cur + lo;
                int idx = lowerBound(nums, target);
                if (idx == nums.length || nums[idx] > end) {
                    ok = false;
                    break;
                }
                cur = nums[idx];
            }
            if (ok) {
                return true;
            }
        }
        return false;
    }

    private int lowerBound(long[] arr, long target) {
        int left = 0, right = arr.length;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(int side, vector<vector<int>>& points, int k) {
        vector<long long> nums;
        for (auto& p : points) {
            int x = p[0];
            int y = p[1];
            if (x == 0) {
                nums.push_back((long long) y);
            } else if (y == side) {
                nums.push_back((long long) side + x);
            } else if (x == side) {
                nums.push_back((long long) side * 3 - y);
            } else {
                nums.push_back((long long) side * 4 - x);
            }
        }
        sort(nums.begin(), nums.end());

        auto check = [&](int lo) -> bool {
            long long total = (long long) side * 4;
            for (long long start : nums) {
                long long end = start + total - lo;
                long long cur = start;
                bool ok = true;
                for (int i = 0; i < k - 1; ++i) {
                    auto it = lower_bound(nums.begin(), nums.end(), cur + lo);
                    if (it == nums.end() || *it > end) {
                        ok = false;
                        break;
                    }
                    cur = *it;
                }
                if (ok) {
                    return true;
                }
            }
            return false;
        };

        int l = 1, r = side;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maxDistance(side int, points [][]int, k int) int {
	nums := make([]int64, 0, len(points))
	for _, p := range points {
		x, y := int64(p[0]), int64(p[1])
		s := int64(side)
		if x == 0 {
			nums = append(nums, y)
		} else if y == s {
			nums = append(nums, s+x)
		} else if x == s {
			nums = append(nums, s*3-y)
		} else {
			nums = append(nums, s*4-x)
		}
	}
	sort.Slice(nums, func(i, j int) bool {
		return nums[i] < nums[j]
	})

	check := func(lo int) bool {
		total := int64(side) * 4
		l64 := int64(lo)
		for _, start := range nums {
			end := start + total - l64
			cur := start
			ok := true
			for i := 0; i < k-1; i++ {
				target := cur + l64
				idx := sort.Search(len(nums), func(i int) bool {
					return nums[i] >= target
				})
				if idx == len(nums) || nums[idx] > end {
					ok = false
					break
				}
				cur = nums[idx]
			}
			if ok {
				return true
			}
		}
		return false
	}

	l, r := 1, side
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function maxDistance(side: number, points: number[][], k: number): number {
    const nums: number[] = [];
    for (const [x, y] of points) {
        if (x === 0) {
            nums.push(y);
        } else if (y === side) {
            nums.push(side + x);
        } else if (x === side) {
            nums.push(side * 3 - y);
        } else {
            nums.push(side * 4 - x);
        }
    }
    nums.sort((a, b) => a - b);

    const check = (lo: number): boolean => {
        const total = side * 4;
        for (const start of nums) {
            const end = start + total - lo;
            let cur = start;
            let ok = true;
            for (let i = 0; i < k - 1; i++) {
                const j = _.sortedIndex(nums, cur + lo);
                if (j === nums.length || nums[j] > end) {
                    ok = false;
                    break;
                }
                cur = nums[j];
            }
            if (ok) {
                return true;
            }
        }
        return false;
    };

    let l = 1,
        r = side;
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
