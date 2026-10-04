---
comments: true
difficulty: Easy
rating: 1203
source: Weekly Contest 387 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3069. Distribute Elements Into Two Arrays I](https://leetcode.com/problems/distribute-elements-into-two-arrays-i)

[中文文档](/solution/3000-3099/3069.Distribute%20Elements%20Into%20Two%20Arrays%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 1</strong> <code>nums</code> gồm <code>n</code> phần tử <strong>không trùng nhau</strong>.</p>

<p>Bạn cần phân phối tất cả phần tử của <code>nums</code> vào hai mảng <code>arr1</code> và <code>arr2</code> bằng <code>n</code> thao tác. Trong thao tác đầu tiên, thêm <code>nums[1]</code> vào <code>arr1</code>. Trong thao tác thứ hai, thêm <code>nums[2]</code> vào <code>arr2</code>. Sau đó, trong thao tác thứ <code>i<sup>th</sup></code>:</p>

<ul>
    <li>Nếu phần tử cuối của <code>arr1</code> <strong>lớn hơn</strong> phần tử cuối của <code>arr2</code>, thêm <code>nums[i]</code> vào <code>arr1</code>. Ngược lại, thêm <code>nums[i]</code> vào <code>arr2</code>.</li>
</ul>

<p>Mảng <code>result</code> được tạo bằng cách nối hai mảng <code>arr1</code> và <code>arr2</code>. Ví dụ, nếu <code>arr1 == [1,2,3]</code> và <code>arr2 == [4,5,6]</code>, thì <code>result = [1,2,3,4,5,6]</code>.</p>

<p>Trả về <em>mảng</em> <code>result</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3]
<strong>Đầu ra:</strong> [2,3,1]
<strong>Giải thích:</strong> Sau 2 thao tác đầu tiên, arr1 = [2] và arr2 = [1].
Trong thao tác thứ 3<sup>rd</sup>, vì phần tử cuối của arr1 lớn hơn phần tử cuối của arr2 (2 &gt; 1), thêm nums[3] vào arr1.
Sau 3 thao tác, arr1 = [2,3] và arr2 = [1].
Do đó, mảng result được tạo bằng cách nối hai mảng là [2,3,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,4,3,8]
<strong>Đầu ra:</strong> [5,3,4,8]
<strong>Giải thích:</strong> Sau 2 thao tác đầu tiên, arr1 = [5] và arr2 = [4].
Trong thao tác thứ 3<sup>rd</sup>, vì phần tử cuối của arr1 lớn hơn phần tử cuối của arr2 (5 &gt; 4), thêm nums[3] vào arr1, khi đó arr1 trở thành [5,3].
Trong thao tác thứ 4<sup>th</sup>, vì phần tử cuối của arr2 lớn hơn phần tử cuối của arr1 (4 &gt; 3), thêm nums[4] vào arr2, khi đó arr2 trở thành [4,8].
Sau 4 thao tác, arr1 = [5,3] và arr2 = [4,8].
Do đó, mảng result được tạo bằng cách nối hai mảng là [5,3,4,8].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= n &lt;= 50</code></li>
    <li><code>1 &lt;= nums[i] &lt;= 100</code></li>
    <li>Tất cả phần tử trong <code>nums</code> đều không trùng nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Hai giá trị đầu tiên khởi tạo hai mảng; các giá trị tiếp theo được đưa vào mảng có phần tử cuối hiện tại lớn hơn. Vì $n \le 50$, chúng ta chỉ cần làm theo quy tắc này.
>
> Chỉ cần quan tâm đến hai phần tử cuối; các phần tử trước đó không còn ảnh hưởng.
>
> Sau khi duyệt xong, nối $\textit{arr}_1+\textit{arr}_2$.

<!-- thinking:end -->

Chúng ta tạo hai mảng $\textit{arr1}$ và $\textit{arr2}$ để lưu các phần tử của $\textit{nums}$. Ban đầu, $\textit{arr1}$ chỉ chứa $\textit{nums[0]}$, còn $\textit{arr2}$ chỉ chứa $\textit{nums[1]}$.

Sau đó, chúng ta duyệt các phần tử của $\textit{nums}$ bắt đầu từ chỉ số $2$. Nếu phần tử cuối của $\textit{arr1}$ lớn hơn phần tử cuối của $\textit{arr2}$, chúng ta thêm phần tử hiện tại vào $\textit{arr1}$; ngược lại, thêm nó vào $\textit{arr2}$.

Cuối cùng, chúng ta thêm các phần tử của $\textit{arr2}$ vào $\textit{arr1}$ rồi trả về $\textit{arr1}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def resultArray(self, nums: List[int]) -> List[int]:
        arr1 = [nums[0]]
        arr2 = [nums[1]]
        for x in nums[2:]:
            if arr1[-1] > arr2[-1]:
                arr1.append(x)
            else:
                arr2.append(x)
        return arr1 + arr2
```

#### Java

```java
class Solution {
    public int[] resultArray(int[] nums) {
        int n = nums.length;
        int[] arr1 = new int[n];
        int[] arr2 = new int[n];
        arr1[0] = nums[0];
        arr2[0] = nums[1];
        int i = 0, j = 0;
        for (int k = 2; k < n; ++k) {
            if (arr1[i] > arr2[j]) {
                arr1[++i] = nums[k];
            } else {
                arr2[++j] = nums[k];
            }
        }
        for (int k = 0; k <= j; ++k) {
            arr1[++i] = arr2[k];
        }
        return arr1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> resultArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> arr1 = {nums[0]};
        vector<int> arr2 = {nums[1]};
        for (int k = 2; k < n; ++k) {
            if (arr1.back() > arr2.back()) {
                arr1.push_back(nums[k]);
            } else {
                arr2.push_back(nums[k]);
            }
        }
        arr1.insert(arr1.end(), arr2.begin(), arr2.end());
        return arr1;
    }
};
```

#### Go

```go
func resultArray(nums []int) []int {
    arr1 := []int{nums[0]}
    arr2 := []int{nums[1]}
    for _, x := range nums[2:] {
        if arr1[len(arr1)-1] > arr2[len(arr2)-1] {
            arr1 = append(arr1, x)
        } else {
            arr2 = append(arr2, x)
        }
    }
    return append(arr1, arr2...)
}
```

#### TypeScript

```ts
function resultArray(nums: number[]): number[] {
    const arr1: number[] = [nums[0]];
    const arr2: number[] = [nums[1]];
    for (const x of nums.slice(2)) {
        if (arr1.at(-1)! > arr2.at(-1)!) {
            arr1.push(x);
        } else {
            arr2.push(x);
        }
    }
    return arr1.concat(arr2);
}
```

#### Rust

```rust
impl Solution {
    pub fn result_array(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut arr1 = vec![nums[0]];
        let mut arr2 = vec![nums[1]];
        for k in 2..n {
            if arr1[arr1.len() - 1] > arr2[arr2.len() - 1] {
                arr1.push(nums[k]);
            } else {
                arr2.push(nums[k]);
            }
        }
        arr1.extend(arr2);
        arr1
    }
}
```

#### C#

```cs
public class Solution {
    public int[] ResultArray(int[] nums) {
        int n = nums.Length;
        var arr1 = new List<int> { nums[0] };
        var arr2 = new List<int> { nums[1] };

        for (int k = 2; k < n; ++k) {
            if (arr1[arr1.Count - 1] > arr2[arr2.Count - 1]) {
                arr1.Add(nums[k]);
            } else {
                arr2.Add(nums[k]);
            }
        }

        arr1.AddRange(arr2);
        return arr1.ToArray();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
