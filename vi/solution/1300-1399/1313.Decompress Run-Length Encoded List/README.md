---
comments: true
difficulty: Easy
rating: 1317
source: Biweekly Contest 17 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1313. Decompress Run-Length Encoded List](https://leetcode.com/problems/decompress-run-length-encoded-list)

[中文文档](/solution/1300-1399/1313.Decompress%20Run-Length%20Encoded%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một danh sách số nguyên <code>nums</code>, biểu diễn một danh sách được nén bằng mã hóa độ dài chuỗi (run-length encoding).</p>

<p>Xét từng cặp phần tử liền kề <code>[freq, val] = [nums[2*i], nums[2*i+1]]</code> (với <code>i &gt;= 0</code>). Với mỗi cặp như vậy, có <code>freq</code> phần tử mang giá trị <code>val</code> được ghép lại thành một danh sách con. Ghép tất cả danh sách con từ trái sang phải để tạo danh sách sau khi giải nén.</p>

<p>Trả về danh sách sau khi giải nén.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> [2,4,4,4]
<strong>Giải thích:</strong> Cặp đầu tiên [1,2] nghĩa là freq = 1 và val = 2, nên ta tạo mảng [2].
Cặp thứ hai [3,4] nghĩa là freq = 3 và val = 4, nên ta tạo [4,4,4].
Cuối cùng, ghép [2] + [4,4,4] ta được [2,4,4,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2,3]
<strong>Đầu ra:</strong> [1,3,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>nums.length % 2 == 0</code></li>
	<li><code><font face="monospace">1 &lt;= nums[i] &lt;= 100</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mã hóa gồm các cặp $(\textit{freq},\textit{val})$; giải nén các cặp này chính là yêu cầu của đề bài. Các cặp không chồng lấn. Duyệt mảng theo từng cặp và lặp lại $\textit{val}$ đúng $\textit{freq}$ lần sẽ tạo được đáp án.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quy trình được mô tả trong đề bài. Duyệt mảng $\textit{nums}$ từ trái sang phải, mỗi lần lấy ra hai số $\textit{freq}$ và $\textit{val}$, sau đó lặp lại $\textit{val}$ $\textit{freq}$ lần và thêm các giá trị đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của mảng $\textit{nums}$. Ta chỉ cần duyệt mảng $\textit{nums}$ một lần. Không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decompressRLElist(self, nums: List[int]) -> List[int]:
        return [nums[i + 1] for i in range(0, len(nums), 2) for _ in range(nums[i])]
```

#### Java

```java
class Solution {
    public int[] decompressRLElist(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i += 2) {
            for (int j = 0; j < nums[i]; ++j) {
                ans.add(nums[i + 1]);
            }
        }
        return ans.stream().mapToInt(i -> i).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> decompressRLElist(vector<int>& nums) {
        vector<int> ans;
        for (int i = 0; i < nums.size(); i += 2) {
            for (int j = 0; j < nums[i]; j++) {
                ans.push_back(nums[i + 1]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func decompressRLElist(nums []int) (ans []int) {
	for i := 1; i < len(nums); i += 2 {
		for j := 0; j < nums[i-1]; j++ {
			ans = append(ans, nums[i])
		}
	}
	return
}
```

#### TypeScript

```ts
function decompressRLElist(nums: number[]): number[] {
    const ans: number[] = [];
    for (let i = 0; i < nums.length; i += 2) {
        for (let j = 0; j < nums[i]; j++) {
            ans.push(nums[i + 1]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn decompress_rl_elist(nums: Vec<i32>) -> Vec<i32> {
        let mut ans = Vec::new();
        let n = nums.len();
        let mut i = 0;
        while i < n {
            let freq = nums[i];
            let val = nums[i + 1];
            for _ in 0..freq {
                ans.push(val);
            }
            i += 2;
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* decompressRLElist(int* nums, int numsSize, int* returnSize) {
    int n = 0;
    for (int i = 0; i < numsSize; i += 2) {
        n += nums[i];
    }
    int* ans = (int*) malloc(n * sizeof(int));
    *returnSize = n;
    int k = 0;
    for (int i = 0; i < numsSize; i += 2) {
        int freq = nums[i];
        int val = nums[i + 1];
        for (int j = 0; j < freq; j++) {
            ans[k++] = val;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
