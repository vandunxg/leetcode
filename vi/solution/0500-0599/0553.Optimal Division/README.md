---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [553. Optimal Division](https://leetcode.com/problems/optimal-division)

[中文文档](/solution/0500-0599/0553.Optimal%20Division/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Các số nguyên liền kề trong <code>nums</code> sẽ được chia theo phép chia số thực.</p>

<ul>
	<li>Ví dụ, với <code>nums = [2,3,4]</code>, ta sẽ tính biểu thức <code>&quot;2/3/4&quot;</code>.</li>
</ul>

<p>Tuy nhiên, bạn có thể thêm dấu ngoặc ở bất kỳ vị trí nào để thay đổi thứ tự thực hiện phép toán. Hãy thêm ngoặc sao cho giá trị của biểu thức sau khi tính là lớn nhất.</p>

<p>Hãy trả về <em>biểu thức dạng chuỗi có giá trị lớn nhất</em>.</p>

<p><strong>Lưu ý:</strong> biểu thức không được chứa dấu ngoặc thừa.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1000,100,10,2]
<strong>Đầu ra:</strong> &quot;1000/(100/10/2)&quot;
<strong>Giải thích:</strong> 1000/(100/10/2) = 1000/((100/10)/2) = 200
Tuy nhiên, cặp ngoặc được in đậm trong &quot;1000/(<strong>(</strong>100/10<strong>)</strong>/2)&quot; là thừa vì không làm thay đổi thứ tự ưu tiên của phép toán.
Vì vậy, cần trả về &quot;1000/(100/10/2)&quot;.
Các trường hợp khác:
1000/(100/10)/2 = 50
1000/(100/(10/2)) = 50
1000/100/10/2 = 0.5
1000/100/(10/2) = 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,4]
<strong>Đầu ra:</strong> &quot;2/(3/4)&quot;
<strong>Giải thích:</strong> (2/(3/4)) = 8/3 = 2.667
Có thể chứng minh rằng dù thử mọi khả năng, ta cũng không thể tạo biểu thức có giá trị lớn hơn 2.667.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10</code></li>
	<li><code>2 &lt;= nums[i] &lt;= 1000</code></li>
	<li>Với mỗi bộ dữ liệu đầu vào, chỉ có một cách chia tối ưu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có phép chia nên dấu ngoặc làm thay đổi thứ tự kết hợp. Muốn giá trị biểu thức lớn nhất, ta cần mẫu số nhỏ nhất có thể.
>
> Với mảng có độ dài $1$ hoặc $2$, không có cách thêm ngoặc nào giúp ích. Nếu mảng dài hơn, lấy số đầu tiên làm tử số và đặt ngoặc quanh phần còn lại dưới dạng chuỗi phép chia: $a_0 / (a_1/a_2/\cdots)$. Cách này làm mẫu số nhỏ nhất; không cần thử mọi cách đặt ngoặc.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def optimalDivision(self, nums: List[int]) -> str:
        n = len(nums)
        if n == 1:
            return str(nums[0])
        if n == 2:
            return f'{nums[0]}/{nums[1]}'
        return f'{nums[0]}/({"/".join(map(str, nums[1:]))})'
```

#### Java

```java
class Solution {
    public String optimalDivision(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return nums[0] + "";
        }
        if (n == 2) {
            return nums[0] + "/" + nums[1];
        }
        StringBuilder ans = new StringBuilder(nums[0] + "/(");
        for (int i = 1; i < n - 1; ++i) {
            ans.append(nums[i] + "/");
        }
        ans.append(nums[n - 1] + ")");
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string optimalDivision(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return to_string(nums[0]);
        if (n == 2) return to_string(nums[0]) + "/" + to_string(nums[1]);
        string ans = to_string(nums[0]) + "/(";
        for (int i = 1; i < n - 1; i++) ans.append(to_string(nums[i]) + "/");
        ans.append(to_string(nums[n - 1]) + ")");
        return ans;
    }
};
```

#### Go

```go
func optimalDivision(nums []int) string {
	n := len(nums)
	if n == 1 {
		return strconv.Itoa(nums[0])
	}
	if n == 2 {
		return fmt.Sprintf("%d/%d", nums[0], nums[1])
	}
	ans := &strings.Builder{}
	ans.WriteString(fmt.Sprintf("%d/(", nums[0]))
	for _, num := range nums[1 : n-1] {
		ans.WriteString(strconv.Itoa(num))
		ans.WriteByte('/')
	}
	ans.WriteString(fmt.Sprintf("%d)", nums[n-1]))
	return ans.String()
}
```

#### TypeScript

```ts
function optimalDivision(nums: number[]): string {
    const n = nums.length;
    const res = nums.join('/');
    if (n > 2) {
        const index = res.indexOf('/') + 1;
        return `${res.slice(0, index)}(${res.slice(index)})`;
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn optimal_division(nums: Vec<i32>) -> String {
        let n = nums.len();
        match n {
            1 => nums[0].to_string(),
            2 => nums[0].to_string() + "/" + &nums[1].to_string(),
            _ => {
                let mut res = nums[0].to_string();
                res.push_str("/(");
                for i in 1..n {
                    res.push_str(&nums[i].to_string());
                    res.push('/');
                }
                res.pop();
                res.push(')');
                res
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
