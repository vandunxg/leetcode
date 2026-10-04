---
comments: true
difficulty: Easy
rating: 1259
source: Weekly Contest 332 Q1
tags:
    - Array
    - Two Pointers
    - Simulation
---

<!-- problem:start -->

# [2562. Find the Array Concatenation Value](https://leetcode.com/problems/find-the-array-concatenation-value)

[中文文档](/solution/2500-2599/2562.Find%20the%20Array%20Concatenation%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>đánh chỉ số từ 0</strong>.</p>

<p><strong>Phép nối</strong> hai số là số được tạo ra bằng cách nối các chữ số biểu diễn của chúng.</p>

<ul>
	<li>Ví dụ, phép nối của <code>15</code> và <code>49</code> là <code>1549</code>.</li>
</ul>

<p><strong>Giá trị nối</strong> của <code>nums</code> ban đầu bằng <code>0</code>. Thực hiện thao tác sau cho đến khi <code>nums</code> trở thành mảng rỗng:</p>

<ul>
	<li>Nếu <code>nums</code> có kích thước lớn hơn một, cộng giá trị phép nối của phần tử đầu tiên và phần tử cuối cùng vào <strong>giá trị nối</strong> của <code>nums</code>, rồi xóa hai phần tử đó khỏi <code>nums</code>. Ví dụ, nếu <code>nums</code> là <code>[1, 2, 4, 5, 6]</code>, ta cộng 16 vào <code>concatenation value</code>.</li>
	<li>Nếu <code>nums</code> chỉ còn một phần tử, cộng giá trị của phần tử đó vào <strong>giá trị nối</strong> của <code>nums</code>, rồi xóa phần tử đó.</li>
</ul>

<p>Trả về <em>giá trị nối của <code>nums</code></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,52,2,4]
<strong>Đầu ra:</strong> 596
<strong>Giải thích:</strong> Trước khi thực hiện thao tác nào, nums là [7,52,2,4] và giá trị nối bằng 0.
  - Ở thao tác đầu tiên:
Ta chọn phần tử đầu tiên, 7, và phần tử cuối cùng, 4.
Phép nối của chúng là 74, ta cộng giá trị này vào giá trị nối, được 74.
Sau đó xóa chúng khỏi nums, nên nums trở thành [52,2].
  - Ở thao tác thứ hai:
Ta chọn phần tử đầu tiên, 52, và phần tử cuối cùng, 2.
Phép nối của chúng là 522, ta cộng giá trị này vào giá trị nối, được 596.
Sau đó xóa chúng khỏi nums, nên nums trở thành mảng rỗng.
Vì giá trị nối là 596 nên đáp án là 596.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,14,13,8,12]
<strong>Đầu ra:</strong> 673
<strong>Giải thích:</strong> Trước khi thực hiện thao tác nào, nums là [5,14,13,8,12] và giá trị nối bằng 0.
  - Ở thao tác đầu tiên:
Ta chọn phần tử đầu tiên, 5, và phần tử cuối cùng, 12.
Phép nối của chúng là 512, ta cộng giá trị này vào giá trị nối, được 512.
Sau đó xóa chúng khỏi nums, nên nums trở thành [14,13,8].
  - Ở thao tác thứ hai:
Ta chọn phần tử đầu tiên, 14, và phần tử cuối cùng, 8.
Phép nối của chúng là 148, ta cộng giá trị này vào giá trị nối, được 660.
Sau đó xóa chúng khỏi nums, nên nums trở thành [13].
  - Ở thao tác thứ ba:
nums chỉ còn một phần tử, nên ta chọn 13 và cộng vào giá trị nối, được 673.
Sau đó xóa phần tử này khỏi nums, nên nums trở thành mảng rỗng.
Vì giá trị nối là 673 nên đáp án là 673.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0; 
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: all 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Liên tục nối hai đầu mảng thành một số nguyên rồi cộng số đó vào kết quả; nếu còn phần tử ở giữa thì cộng trực tiếp phần tử đó. Vì $n$ nhỏ, ta có thể dùng hai con trỏ và phép nối chuỗi để cài đặt đúng theo đề bài.

<!-- thinking:end -->

Bắt đầu từ hai đầu mảng, mỗi lần ta lấy ra một phần tử ở mỗi đầu, nối chúng lại rồi cộng kết quả nối vào đáp án. Lặp lại quá trình này cho đến khi mảng rỗng.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(\log M)$. Trong đó, $n$ và $M$ lần lượt là độ dài mảng và giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheArrayConcVal(self, nums: List[int]) -> int:
        ans = 0
        i, j = 0, len(nums) - 1
        while i < j:
            ans += int(str(nums[i]) + str(nums[j]))
            i, j = i + 1, j - 1
        if i == j:
            ans += nums[i]
        return ans
```

#### Java

```java
class Solution {
    public long findTheArrayConcVal(int[] nums) {
        long ans = 0;
        int i = 0, j = nums.length - 1;
        for (; i < j; ++i, --j) {
            ans += Integer.parseInt(nums[i] + "" + nums[j]);
        }
        if (i == j) {
            ans += nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long findTheArrayConcVal(vector<int>& nums) {
        long long ans = 0;
        int i = 0, j = nums.size() - 1;
        for (; i < j; ++i, --j) {
            ans += stoi(to_string(nums[i]) + to_string(nums[j]));
        }
        if (i == j) {
            ans += nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func findTheArrayConcVal(nums []int) (ans int64) {
	i, j := 0, len(nums)-1
	for ; i < j; i, j = i+1, j-1 {
		x, _ := strconv.Atoi(strconv.Itoa(nums[i]) + strconv.Itoa(nums[j]))
		ans += int64(x)
	}
	if i == j {
		ans += int64(nums[i])
	}
	return
}
```

#### TypeScript

```ts
function findTheArrayConcVal(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    let i = 0;
    let j = n - 1;
    while (i < j) {
        ans += Number(`${nums[i]}${nums[j]}`);
        i++;
        j--;
    }
    if (i === j) {
        ans += nums[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_array_conc_val(nums: Vec<i32>) -> i64 {
        let n = nums.len();
        let mut ans = 0;
        let mut i = 0;
        let mut j = n - 1;
        while i < j {
            ans += format!("{}{}", nums[i], nums[j]).parse::<i64>().unwrap();
            i += 1;
            j -= 1;
        }
        if i == j {
            ans += nums[i] as i64;
        }
        ans
    }
}
```

#### C

```c
int getLen(int num) {
    int res = 0;
    while (num) {
        num /= 10;
        res++;
    }
    return res;
}

long long findTheArrayConcVal(int* nums, int numsSize) {
    long long ans = 0;
    int i = 0;
    int j = numsSize - 1;
    while (i < j) {
        ans += nums[i] * pow(10, getLen(nums[j])) + nums[j];
        i++;
        j--;
    }
    if (i == j) {
        ans += nums[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 thu hẹp một cặp con trỏ. Ghép chỉ số $i$ với $n-1-i$ cho đến $n/2$, rồi cộng phần tử ở giữa nếu $n$ lẻ, cũng chính là quá trình đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
impl Solution {
    pub fn find_the_array_conc_val(nums: Vec<i32>) -> i64 {
        let mut ans = 0;
        let mut n = nums.len();

        for i in 0..n / 2 {
            ans += format!("{}{}", nums[i], nums[n - i - 1])
                .parse::<i64>()
                .unwrap();
        }

        if n % 2 != 0 {
            ans += nums[n / 2] as i64;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
