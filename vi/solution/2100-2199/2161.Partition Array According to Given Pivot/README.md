---
comments: true
difficulty: Medium
rating: 1337
source: Biweekly Contest 71 Q2
tags:
    - Array
    - Two Pointers
    - Simulation
---

<!-- problem:start -->

# [2161. Partition Array According to Given Pivot](https://leetcode.com/problems/partition-array-according-to-given-pivot)

[中文文档](/solution/2100-2199/2161.Partition%20Array%20According%20to%20Given%20Pivot/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên <code>pivot</code>. Hãy sắp xếp lại <code>nums</code> sao cho thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Mọi phần tử nhỏ hơn <code>pivot</code> đều xuất hiện <strong>trước</strong> mọi phần tử lớn hơn <code>pivot</code>.</li>
	<li>Mọi phần tử bằng <code>pivot</code> đều xuất hiện <strong>ở giữa</strong> các phần tử nhỏ hơn và lớn hơn <code>pivot</code>.</li>
	<li><strong>Thứ tự tương đối</strong> của các phần tử nhỏ hơn <code>pivot</code> và các phần tử lớn hơn <code>pivot</code> được giữ nguyên.
	<ul>
		<li>Cụ thể hơn, xét mọi <code>p<sub>i</sub></code>, <code>p<sub>j</sub></code>, trong đó <code>p<sub>i</sub></code> là vị trí mới của phần tử <code>i<sup>th</sup></code> và <code>p<sub>j</sub></code> là vị trí mới của phần tử <code>j<sup>th</sup></code>. Nếu <code>i &lt; j</code> và <strong>cả hai</strong> phần tử đều nhỏ hơn (<em>hoặc lớn hơn</em>) <code>pivot</code>, thì <code>p<sub>i</sub> &lt; p<sub>j</sub></code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về <code>nums</code><em> sau khi sắp xếp lại.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,12,5,10,14,3,10], pivot = 10
<strong>Đầu ra:</strong> [9,5,3,10,10,12,14]
<strong>Giải thích:</strong>
Các phần tử 9, 5 và 3 nhỏ hơn pivot nên nằm ở phía bên trái của mảng.
Các phần tử 12 và 14 lớn hơn pivot nên nằm ở phía bên phải của mảng.
Thứ tự tương đối của các phần tử nhỏ hơn và lớn hơn pivot cũng được giữ nguyên. [9, 5, 3] và [12, 14] lần lượt là các thứ tự đó.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-3,4,3,2], pivot = 2
<strong>Đầu ra:</strong> [-3,2,4,3]
<strong>Giải thích:</strong>
Phần tử -3 nhỏ hơn pivot nên nằm ở phía bên trái của mảng.
Các phần tử 4 và 3 lớn hơn pivot nên nằm ở phía bên phải của mảng.
Thứ tự tương đối của các phần tử nhỏ hơn và lớn hơn pivot cũng được giữ nguyên. [-3] và [4, 3] lần lượt là các thứ tự đó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>pivot</code> bằng một phần tử của <code>nums</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Phân hoạch theo $\textit{pivot}$ thành ba phần nhỏ hơn, bằng và lớn hơn, đồng thời giữ nguyên thứ tự trong mỗi phần. Chỉ cần thực hiện việc tách ổn định trong một lượt duyệt.
>
> Thu thập ba danh sách theo thứ tự gặp phần tử rồi nối chúng lại.
>
> Bộ nhớ bổ sung có độ dài tuyến tính.

<!-- thinking:end -->

Ta có thể duyệt qua mảng $\textit{nums}$, lần lượt tìm tất cả phần tử nhỏ hơn $\textit{pivot}$, tất cả phần tử bằng $\textit{pivot}$ và tất cả phần tử lớn hơn $\textit{pivot}$, sau đó nối chúng theo thứ tự mà đề bài yêu cầu.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Không tính phần bộ nhớ được dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pivotArray(self, nums: List[int], pivot: int) -> List[int]:
        a, b, c = [], [], []
        for x in nums:
            if x < pivot:
                a.append(x)
            elif x == pivot:
                b.append(x)
            else:
                c.append(x)
        return a + b + c
```

#### Java

```java
class Solution {
    public int[] pivotArray(int[] nums, int pivot) {
        int n = nums.length;
        int[] ans = new int[n];
        int k = 0;
        for (int x : nums) {
            if (x < pivot) {
                ans[k++] = x;
            }
        }
        for (int x : nums) {
            if (x == pivot) {
                ans[k++] = x;
            }
        }
        for (int x : nums) {
            if (x > pivot) {
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
    vector<int> pivotArray(vector<int>& nums, int pivot) {
        vector<int> ans;
        for (int& x : nums) {
            if (x < pivot) {
                ans.push_back(x);
            }
        }
        for (int& x : nums) {
            if (x == pivot) {
                ans.push_back(x);
            }
        }
        for (int& x : nums) {
            if (x > pivot) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func pivotArray(nums []int, pivot int) []int {
	var ans []int
	for _, x := range nums {
		if x < pivot {
			ans = append(ans, x)
		}
	}
	for _, x := range nums {
		if x == pivot {
			ans = append(ans, x)
		}
	}
	for _, x := range nums {
		if x > pivot {
			ans = append(ans, x)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function pivotArray(nums: number[], pivot: number): number[] {
    const ans: number[] = [];
    for (const x of nums) {
        if (x < pivot) {
            ans.push(x);
        }
    }
    for (const x of nums) {
        if (x === pivot) {
            ans.push(x);
        }
    }
    for (const x of nums) {
        if (x > pivot) {
            ans.push(x);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng ba bộ đệm. Khởi tạo đáp án bằng $\textit{pivot}$ giúp tránh phải ghi phần bằng giá trị này.
>
> Điền các giá trị nhỏ hơn từ bên trái và các giá trị lớn hơn từ bên phải; phần giữa giữ nguyên giá trị bằng. Hai lượt duyệt ngược chiều giữ nguyên thứ tự tương đối của mỗi phía.
>
> Biến thể điền trực tiếp này được minh họa bằng TypeScript / JavaScript.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function pivotArray(nums: number[], pivot: number): number[] {
    const n = nums.length;
    const res = Array(n).fill(pivot);

    for (let i = 0, l = 0, j = n - 1, r = n - 1; i < n; i++, j--) {
        if (nums[i] < pivot) res[l++] = nums[i];
        if (nums[j] > pivot) res[r--] = nums[j];
    }

    return res;
}
```

#### JavaScript

```js
function pivotArray(nums, pivot) {
    const n = nums.length;
    const res = Array(n).fill(pivot);

    for (let i = 0, l = 0, j = n - 1, r = n - 1; i < n; i++, j--) {
        if (nums[i] < pivot) res[l++] = nums[i];
        if (nums[j] > pivot) res[r--] = nums[j];
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
