---
comments: true
difficulty: Easy
rating: 1163
source: Weekly Contest 415 Q1
tags:
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [3289. The Two Sneaky Numbers of Digitville](https://leetcode.com/problems/the-two-sneaky-numbers-of-digitville)

[Tài liệu tiếng Trung](/solution/3200-3299/3289.The%20Two%20Sneaky%20Numbers%20of%20Digitville/README.md)

## Mô tả

<!-- description:start -->

<p>Ở thị trấn Digitville, có một danh sách số gọi là <code>nums</code> chứa các số nguyên từ <code>0</code> đến <code>n - 1</code>. Mỗi số đáng lẽ phải xuất hiện <strong>đúng một lần</strong> trong danh sách, nhưng có <strong>hai</strong> số tinh nghịch xuất hiện <em>thêm một lần</em>, khiến danh sách dài hơn bình thường.<!-- notionvc: c37cfb04-95eb-4273-85d5-3c52d0525b95 --></p>

<p>Với vai trò thám tử của thị trấn, nhiệm vụ của bạn là tìm hai số tinh nghịch này. Hãy trả về một mảng có kích thước <strong>two</strong> chứa hai số đó (theo <em>bất kỳ thứ tự nào</em>), để Digitville có thể trở lại yên bình.<!-- notionvc: 345db5be-c788-4828-9836-eefed31c982f --></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số 0 và 1 đều xuất hiện hai lần trong mảng.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,3,2,1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3]</span></p>

<p><strong>Giải thích: </strong></p>

<p>Các số 2 và 3 đều xuất hiện hai lần trong mảng.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,1,5,4,3,4,6,0,9,5,8,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,5]</span></p>

<p><strong>Giải thích: </strong></p>

<p>Các số 4 và 5 đều xuất hiện hai lần trong mảng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li data-stringify-border="0" data-stringify-indent="1"><code>2 &lt;= n &lt;= 100</code></li>
	<li data-stringify-border="0" data-stringify-indent="1"><code>nums.length == n + 2</code></li>
	<li data-stringify-border="0" data-stringify-indent="1"><code data-stringify-type="code">0 &lt;= nums[i] &lt; n</code></li>
	<li data-stringify-border="0" data-stringify-indent="1">Dữ liệu đầu vào được tạo sao cho <code>nums</code> chứa <strong>chính xác</strong> hai phần tử bị lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mảng chứa mỗi số trong $0\ldots n-1$ một lần, cộng thêm hai lần xuất hiện dư. Vì $n\le 100$, chỉ cần đếm các giá trị có tần suất bằng $2$.
>
> Một map hoặc một mảng có độ dài $n$ sẽ ghi lại số lần xuất hiện; hai giá trị có số lần đếm bằng $2$ chính là đáp án.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi số.

Duyệt qua mảng $\textit{nums}$, khi một số xuất hiện lần thứ hai thì thêm số đó vào mảng đáp án.

Sau khi duyệt xong, trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSneakyNumbers(self, nums: List[int]) -> List[int]:
        cnt = Counter(nums)
        return [x for x, v in cnt.items() if v == 2]
```

#### Java

```java
class Solution {
    public int[] getSneakyNumbers(int[] nums) {
        int[] ans = new int[2];
        int[] cnt = new int[100];
        int k = 0;
        for (int x : nums) {
            if (++cnt[x] == 2) {
                ans[k++] = x;
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
    vector<int> getSneakyNumbers(vector<int>& nums) {
        vector<int> ans;
        int cnt[100]{};
        for (int x : nums) {
            if (++cnt[x] == 2) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getSneakyNumbers(nums []int) (ans []int) {
	cnt := [100]int{}
	for _, x := range nums {
		cnt[x]++
		if cnt[x] == 2 {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function getSneakyNumbers(nums: number[]): number[] {
    const ans: number[] = [];
    const cnt: number[] = Array(100).fill(0);
    for (const x of nums) {
        if (++cnt[x] > 1) {
            ans.push(x);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn get_sneaky_numbers(nums: Vec<i32>) -> Vec<i32> {
        let mut cnt = HashMap::new();
        for x in nums {
            *cnt.entry(x).or_insert(0) += 1;
        }
        let mut ans = Vec::new();
        for (x, v) in cnt {
            if v == 2 {
                ans.push(x);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng bộ nhớ phụ tuyến tính. Phép XOR của hai số lặp lại bằng phép XOR của mảng với $0\ldots n-1$. Bit cao nhất tại đó chúng khác nhau sẽ chia mọi số thành hai nhóm; phép XOR trong mỗi nhóm sẽ tách riêng từng đáp án với bộ nhớ phụ $O(1)$.

<!-- thinking:end -->

Gọi độ dài của mảng $\textit{nums}$ là $n + 2$, trong đó chứa các số nguyên từ $0$ đến $n - 1$, và có hai số xuất hiện hai lần.

Ta có thể tìm hai số này bằng các phép toán XOR. Trước tiên, ta thực hiện phép XOR trên tất cả các số trong mảng $\textit{nums}$ và các số nguyên từ $0$ đến $n - 1$. Kết quả là giá trị XOR của hai số trùng lặp, ký hiệu là $xx$.

Tiếp theo, ta có thể tìm một số đặc điểm của hai số này thông qua $xx$ và tách chúng ra. Các bước cụ thể như sau:

1. Tìm vị trí của bit thấp nhất hoặc cao nhất có giá trị $1$ trong biểu diễn nhị phân của $xx$, ký hiệu là $k$. Vị trí này cho biết hai số khác nhau tại bit đó.
2. Dựa trên giá trị của bit thứ $k$, chia các số trong mảng $\textit{nums}$ và các số nguyên từ $0$ đến $n - 1$ thành hai nhóm: một nhóm có giá trị $0$ tại bit thứ $k$, nhóm còn lại có giá trị $1$ tại bit thứ $k$. Sau đó thực hiện phép XOR riêng trên từng nhóm, kết quả thu được là hai số trùng lặp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSneakyNumbers(self, nums: List[int]) -> List[int]:
        n = len(nums) - 2
        xx = nums[n] ^ nums[n + 1]
        for i in range(n):
            xx ^= i ^ nums[i]
        k = xx.bit_length() - 1
        ans = [0, 0]
        for x in nums:
            ans[x >> k & 1] ^= x
        for i in range(n):
            ans[i >> k & 1] ^= i
        return ans
```

#### Java

```java
class Solution {
    public int[] getSneakyNumbers(int[] nums) {
        int n = nums.length - 2;
        int xx = nums[n] ^ nums[n + 1];
        for (int i = 0; i < n; ++i) {
            xx ^= i ^ nums[i];
        }
        int k = Integer.numberOfTrailingZeros(xx);
        int[] ans = new int[2];
        for (int x : nums) {
            ans[x >> k & 1] ^= x;
        }
        for (int i = 0; i < n; ++i) {
            ans[i >> k & 1] ^= i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getSneakyNumbers(vector<int>& nums) {
        int n = nums.size() - 2;
        int xx = nums[n] ^ nums[n + 1];
        for (int i = 0; i < n; ++i) {
            xx ^= i ^ nums[i];
        }
        int k = __builtin_ctz(xx);
        vector<int> ans(2);
        for (int x : nums) {
            ans[(x >> k) & 1] ^= x;
        }
        for (int i = 0; i < n; ++i) {
            ans[(i >> k) & 1] ^= i;
        }
        return ans;
    }
};
```

#### Go

```go
func getSneakyNumbers(nums []int) []int {
	n := len(nums) - 2
	xx := nums[n] ^ nums[n+1]
	for i := 0; i < n; i++ {
		xx ^= i ^ nums[i]
	}
	k := bits.TrailingZeros(uint(xx))
	ans := make([]int, 2)
	for _, x := range nums {
		ans[(x>>k)&1] ^= x
	}
	for i := 0; i < n; i++ {
		ans[(i>>k)&1] ^= i
	}
	return ans
}
```

#### TypeScript

```ts
function getSneakyNumbers(nums: number[]): number[] {
    const n = nums.length - 2;
    let xx = nums[n] ^ nums[n + 1];
    for (let i = 0; i < n; ++i) {
        xx ^= i ^ nums[i];
    }
    const k = Math.clz32(xx & -xx) ^ 31;
    const ans = [0, 0];
    for (const x of nums) {
        ans[(x >> k) & 1] ^= x;
    }
    for (let i = 0; i < n; ++i) {
        ans[(i >> k) & 1] ^= i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn get_sneaky_numbers(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len() as i32 - 2;
        let mut xx = nums[n as usize] ^ nums[(n + 1) as usize];
        for i in 0..n {
            xx ^= i ^ nums[i as usize];
        }
        let k = xx.trailing_zeros();
        let mut ans = vec![0, 0];
        for &x in &nums {
            ans[((x >> k) & 1) as usize] ^= x;
        }
        for i in 0..n {
            ans[((i >> k) & 1) as usize] ^= i;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
