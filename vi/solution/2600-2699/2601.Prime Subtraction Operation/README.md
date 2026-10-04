---
comments: true
difficulty: Medium
rating: 1779
source: Weekly Contest 338 Q2
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
    - Number Theory
---

<!-- problem:start -->

# [2601. Prime Subtraction Operation](https://leetcode.com/problems/prime-subtraction-operation)

[中文文档](/solution/2600-2699/2601.Prime%20Subtraction%20Operation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>, <strong>được đánh chỉ số từ 0</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> chưa từng được chọn, chọn một số nguyên tố <code>p</code> <strong>nhỏ hơn nghiêm ngặt</strong> <code>nums[i]</code>, rồi trừ <code>p</code> khỏi <code>nums[i]</code>.</li>
</ul>

<p>Trả về <em>true nếu có thể biến <code>nums</code> thành một mảng tăng nghiêm ngặt bằng thao tác trên, và false nếu không thể.</em></p>

<p>Một <strong>mảng tăng nghiêm ngặt</strong> là mảng mà mỗi phần tử đều lớn hơn phần tử đứng ngay trước nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,9,6,10]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Trong thao tác đầu tiên: Chọn i = 0 và p = 3, sau đó trừ 3 khỏi nums[0], khi đó nums trở thành [1,9,6,10].
Trong thao tác thứ hai: Chọn i = 1, p = 7, trừ 7 khỏi nums[1], khi đó nums trở thành [1,2,6,10].
Sau thao tác thứ hai, nums đã được sắp xếp theo thứ tự tăng nghiêm ngặt, nên đáp án là true.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,8,11,12]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Ban đầu nums đã được sắp xếp theo thứ tự tăng nghiêm ngặt, nên ta không cần thực hiện thao tác nào.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,8,3]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Có thể chứng minh rằng không có cách nào thực hiện các thao tác để sắp xếp nums theo thứ tự tăng nghiêm ngặt, nên đáp án là false.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code><font face="monospace">nums.length == n</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý số nguyên tố + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chỉ số chỉ có thể bị trừ một số nguyên tố nhiều nhất một lần, và mảng phải trở thành mảng tăng nghiêm ngặt. Greedy từ trái sang phải có thể làm một giá trị giảm quá nhiều, khiến các vị trí phía sau khó xử lý. Việc liệt kê các số nguyên tố tại mỗi chỉ số là khả thi với $n \le 1000$ và các giá trị không vượt quá $1000$, nhưng hướng duyệt vẫn rất quan trọng.
>
> Giá trị ngoài cùng bên phải $nums[n-1]$ có nhiều khoảng trống hơn khi nó lớn, nên ta xử lý từ phải sang trái. Khi $nums[i] \ge nums[i+1]$, ta phải trừ một số nguyên tố lớn hơn nghiêm ngặt $nums[i]-nums[i+1]$ và nhỏ hơn $nums[i]$, đồng thời chọn số nguyên tố nhỏ nhất như vậy để phần bên trái còn nhiều khoảng trống nhất.
>
> Vì vậy, ta dùng sàng để tìm các số nguyên tố không vượt quá $1000$ và lưu chúng vào $p$, sau đó tìm kiếm nhị phân số nguyên tố nhỏ nhất lớn hơn cận dưới đó. Nếu không tồn tại số nào, mảng không thể được biến thành mảng tăng nghiêm ngặt.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý tất cả các số nguyên tố không vượt quá $1000$ và lưu chúng vào mảng $p$.

Với mỗi phần tử $nums[i]$ trong mảng $nums$, ta cần tìm một số nguyên tố $p[j]$ sao cho $p[j] \gt nums[i] - nums[i + 1]$ và $p[j]$ nhỏ nhất có thể. Nếu không tồn tại số nguyên tố như vậy, nghĩa là không thể biến mảng thành mảng tăng nghiêm ngặt bằng các thao tác trừ, khi đó trả về `false`. Nếu tồn tại, ta trừ $p[j]$ khỏi $nums[i]$ rồi tiếp tục xử lý phần tử kế tiếp.

Nếu tất cả phần tử trong $nums$ đều được xử lý, nghĩa là có thể biến mảng thành mảng tăng nghiêm ngặt bằng các thao tác trừ, khi đó trả về `true`.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def primeSubOperation(self, nums: List[int]) -> bool:
        p = []
        for i in range(2, max(nums)):
            for j in p:
                if i % j == 0:
                    break
            else:
                p.append(i)

        n = len(nums)
        for i in range(n - 2, -1, -1):
            if nums[i] < nums[i + 1]:
                continue
            j = bisect_right(p, nums[i] - nums[i + 1])
            if j == len(p) or p[j] >= nums[i]:
                return False
            nums[i] -= p[j]
        return True
```

#### Java

```java
class Solution {
    public boolean primeSubOperation(int[] nums) {
        List<Integer> p = new ArrayList<>();
        for (int i = 2; i <= 1000; ++i) {
            boolean ok = true;
            for (int j : p) {
                if (i % j == 0) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                p.add(i);
            }
        }
        int n = nums.length;
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] < nums[i + 1]) {
                continue;
            }
            int j = search(p, nums[i] - nums[i + 1]);
            if (j == p.size() || p.get(j) >= nums[i]) {
                return false;
            }
            nums[i] -= p.get(j);
        }
        return true;
    }

    private int search(List<Integer> nums, int x) {
        int l = 0, r = nums.size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums.get(mid) > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool primeSubOperation(vector<int>& nums) {
        vector<int> p;
        for (int i = 2; i <= 1000; ++i) {
            bool ok = true;
            for (int j : p) {
                if (i % j == 0) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                p.push_back(i);
            }
        }
        int n = nums.size();
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] < nums[i + 1]) {
                continue;
            }
            int j = upper_bound(p.begin(), p.end(), nums[i] - nums[i + 1]) - p.begin();
            if (j == p.size() || p[j] >= nums[i]) {
                return false;
            }
            nums[i] -= p[j];
        }
        return true;
    }
};
```

#### Go

```go
func primeSubOperation(nums []int) bool {
	p := []int{}
	for i := 2; i <= 1000; i++ {
		ok := true
		for _, j := range p {
			if i%j == 0 {
				ok = false
				break
			}
		}
		if ok {
			p = append(p, i)
		}
	}
	for i := len(nums) - 2; i >= 0; i-- {
		if nums[i] < nums[i+1] {
			continue
		}
		j := sort.SearchInts(p, nums[i]-nums[i+1]+1)
		if j == len(p) || p[j] >= nums[i] {
			return false
		}
		nums[i] -= p[j]
	}
	return true
}
```

#### TypeScript

```ts
function primeSubOperation(nums: number[]): boolean {
    const p: number[] = [];
    for (let i = 2; i <= 1000; ++i) {
        let ok = true;
        for (const j of p) {
            if (i % j === 0) {
                ok = false;
                break;
            }
        }
        if (ok) {
            p.push(i);
        }
    }
    const search = (x: number): number => {
        let l = 0;
        let r = p.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (p[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const n = nums.length;
    for (let i = n - 2; i >= 0; --i) {
        if (nums[i] < nums[i + 1]) {
            continue;
        }
        const j = search(nums[i] - nums[i + 1]);
        if (j === p.length || p[j] >= nums[i]) {
            return false;
        }
        nums[i] -= p[j];
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tiền xử lý số nguyên tố

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tìm kiếm nhị phân trên bảng số nguyên tố ở mỗi lần xử lý. Vì các giá trị không vượt quá $1000$, ta có thể truy cập trực tiếp: đặt $p[x]$ là số nguyên tố nhỏ nhất lớn hơn hoặc bằng $x$. Khi đó, cận dưới $nums[i]-nums[i+1]$ chỉ cần một lần truy cập mảng, loại bỏ thao tác tìm kiếm.
>
> Phần còn lại không thay đổi: vẫn xử lý từ phải sang trái và vẫn trừ đi số nguyên tố nhỏ nhất đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function primeSubOperation(nums: number[]): boolean {
    const p: number[] = [];
    const max = Math.max(...nums);

    for (let i = 2; i < max; i++) {
        let isPrime = true;

        for (const x of p) {
            if (i % x === 0) {
                isPrime = false;
                break;
            }
        }

        while (isPrime && p.length <= i) {
            p.push(i);
        }
    }

    for (let i = nums.length - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) continue;

        const [x, next] = [nums[i], nums[i + 1]];
        const prime = p[x - next + 1];

        if (!prime || prime >= x) return false;
        nums[i] -= prime;
    }

    return true;
}
```

#### JavaScript

```js
function primeSubOperation(nums) {
    const p = [];
    const max = Math.max(...nums);

    for (let i = 2; i < max; i++) {
        let isPrime = true;

        for (const x of p) {
            if (i % x === 0) {
                isPrime = false;
                break;
            }
        }

        while (isPrime && p.length <= i) {
            p.push(i);
        }
    }

    for (let i = nums.length - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) continue;

        const [x, next] = [nums[i], nums[i + 1]];
        const prime = p[x - next + 1];

        if (!prime || prime >= x) return false;
        nums[i] -= prime;
    }

    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
