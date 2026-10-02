---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [969. Pancake Sorting](https://leetcode.com/problems/pancake-sorting)

[中文文档](/solution/0900-0999/0969.Pancake%20Sorting/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy sắp xếp mảng bằng cách thực hiện một loạt <strong>lần lật pancake</strong>.</p>

<p>Mỗi lần lật pancake, ta thực hiện các bước sau:</p>

<ul>
	<li>Chọn số nguyên <code>k</code> thỏa mãn <code>1 &lt;= k &lt;= arr.length</code>.</li>
	<li>Đảo ngược mảng con <code>arr[0...k-1]</code> (đánh chỉ số từ <strong>0</strong>).</li>
</ul>

<p>Ví dụ, với <code>arr = [3,2,1,4]</code>, nếu lật pancake với <code>k = 3</code>, ta đảo ngược mảng con <code>[3,2,1]</code>. Sau lần lật tại <code>k = 3</code>, ta được <code>arr = [<u>1</u>,<u>2</u>,<u>3</u>,4]</code>.</p>

<p>Trả về <em>mảng gồm các giá trị </em><code>k</code><em> tương ứng với một chuỗi lần lật pancake giúp sắp xếp </em><code>arr</code>. Mọi đáp án hợp lệ sắp xếp được mảng trong không quá <code>10 * arr.length</code> lần lật đều được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,2,4,1]
<strong>Đầu ra:</strong> [4,2,4,3]
<strong>Giải thích: </strong>
Ta thực hiện 4 lần lật pancake với các giá trị k lần lượt là 4, 2, 4 và 3.
Trạng thái ban đầu: arr = [3, 2, 4, 1]
Sau lần lật thứ nhất (k = 4): arr = [<u>1</u>, <u>4</u>, <u>2</u>, <u>3</u>]
Sau lần lật thứ hai (k = 2): arr = [<u>4</u>, <u>1</u>, 2, 3]
Sau lần lật thứ ba (k = 4): arr = [<u>3</u>, <u>2</u>, <u>1</u>, <u>4</u>]
Sau lần lật thứ tư (k = 3): arr = [<u>1</u>, <u>2</u>, <u>3</u>, 4], mảng đã được sắp xếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3]
<strong>Đầu ra:</strong> []
<strong>Giải thích: </strong>Mảng đầu vào đã được sắp xếp nên không cần lật lần nào.
Lưu ý, các đáp án khác như [3, 3] cũng được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 100</code></li>
	<li><code>1 &lt;= arr[i] &lt;= arr.length</code></li>
	<li>Tất cả số nguyên trong <code>arr</code> đều khác nhau (tức là <code>arr</code> là một hoán vị của các số nguyên từ <code>1</code> đến <code>arr.length</code>).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một lần lật pancake đảo ngược một prefix. Vì mảng là hoán vị của $1..n$, ta có thể đặt các giá trị từ lớn xuống nhỏ: lật để đưa giá trị cần đặt lên đầu, rồi lật lần nữa để đưa nó về vị trí cuối cùng trong phần chưa sắp xếp. Ghi lại độ dài của hai prefix được lật. Không thay đổi suffix đã được sắp xếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pancakeSort(self, arr: List[int]) -> List[int]:
        def reverse(arr, j):
            i = 0
            while i < j:
                arr[i], arr[j] = arr[j], arr[i]
                i, j = i + 1, j - 1

        n = len(arr)
        ans = []
        for i in range(n - 1, 0, -1):
            j = i
            while j > 0 and arr[j] != i + 1:
                j -= 1
            if j < i:
                if j > 0:
                    ans.append(j + 1)
                    reverse(arr, j)
                ans.append(i + 1)
                reverse(arr, i)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> pancakeSort(int[] arr) {
        int n = arr.length;
        List<Integer> ans = new ArrayList<>();
        for (int i = n - 1; i > 0; --i) {
            int j = i;
            for (; j > 0 && arr[j] != i + 1; --j)
                ;
            if (j < i) {
                if (j > 0) {
                    ans.add(j + 1);
                    reverse(arr, j);
                }
                ans.add(i + 1);
                reverse(arr, i);
            }
        }
        return ans;
    }

    private void reverse(int[] arr, int j) {
        for (int i = 0; i < j; ++i, --j) {
            int t = arr[i];
            arr[i] = arr[j];
            arr[j] = t;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> pancakeSort(vector<int>& arr) {
        int n = arr.size();
        vector<int> ans;
        for (int i = n - 1; i > 0; --i) {
            int j = i;
            for (; j > 0 && arr[j] != i + 1; --j)
                ;
            if (j == i) continue;
            if (j > 0) {
                ans.push_back(j + 1);
                reverse(arr.begin(), arr.begin() + j + 1);
            }
            ans.push_back(i + 1);
            reverse(arr.begin(), arr.begin() + i + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func pancakeSort(arr []int) []int {
	var ans []int
	n := len(arr)
	reverse := func(j int) {
		for i := 0; i < j; i, j = i+1, j-1 {
			arr[i], arr[j] = arr[j], arr[i]
		}
	}
	for i := n - 1; i > 0; i-- {
		j := i
		for ; j > 0 && arr[j] != i+1; j-- {
		}
		if j < i {
			if j > 0 {
				ans = append(ans, j+1)
				reverse(j)
			}
			ans = append(ans, i+1)
			reverse(i)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function pancakeSort(arr: number[]): number[] {
    let ans = [];
    for (let n = arr.length; n > 1; n--) {
        let index = 0;
        for (let i = 1; i < n; i++) {
            if (arr[i] >= arr[index]) {
                index = i;
            }
        }
        if (index == n - 1) continue;
        reverse(arr, index);
        reverse(arr, n - 1);
        ans.push(index + 1);
        ans.push(n);
    }
    return ans;
}

function reverse(nums: Array<number>, end: number): void {
    for (let i = 0, j = end; i < j; i++, j--) {
        [nums[i], nums[j]] = [nums[j], nums[i]];
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn pancake_sort(mut arr: Vec<i32>) -> Vec<i32> {
        let mut res = vec![];
        for n in (1..arr.len()).rev() {
            let mut max_idx = 0;
            for idx in 0..=n {
                if arr[max_idx] < arr[idx] {
                    max_idx = idx;
                }
            }
            if max_idx != n {
                if max_idx != 0 {
                    arr[..=max_idx].reverse();
                    res.push((max_idx as i32) + 1);
                }
                arr[..=n].reverse();
                res.push((n as i32) + 1);
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
