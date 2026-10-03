---
comments: true
difficulty: Hard
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1842. Next Palindrome Using Same Digits 🔒](https://leetcode.com/problems/next-palindrome-using-same-digits)

[中文文档](/solution/1800-1899/1842.Next%20Palindrome%20Using%20Same%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi số <code>num</code> biểu diễn một <strong>số đối xứng</strong> rất lớn.</p>

<p>Trả về<em> <strong>số đối xứng nhỏ nhất lớn hơn </strong></em><code>num</code><em> có thể tạo ra bằng cách sắp xếp lại các chữ số của nó. Nếu không tồn tại số đối xứng như vậy, trả về chuỗi rỗng </em><code>&quot;&quot;</code>.</p>

<p><strong>Số đối xứng</strong> là số đọc từ trái sang phải giống với khi đọc từ phải sang trái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;1221&quot;
<strong>Đầu ra:</strong> &quot;2112&quot;
<strong>Giải thích:</strong>&nbsp;Số đối xứng tiếp theo lớn hơn &quot;1221&quot; là &quot;2112&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;32123&quot;
<strong>Đầu ra:</strong> &quot;&quot;
<strong>Giải thích:</strong>&nbsp;Không thể tạo số đối xứng nào lớn hơn &quot;32123&quot; bằng cách sắp xếp lại các chữ số.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;45544554&quot;
<strong>Đầu ra:</strong> &quot;54455445&quot;
<strong>Giải thích:</strong> Số đối xứng tiếp theo lớn hơn &quot;45544554&quot; là &quot;54455445&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10<sup>5</sup></code></li>
	<li><code>num</code> là một <strong>số đối xứng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm hoán vị kế tiếp của nửa đầu

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần số đối xứng lớn hơn tiếp theo, sử dụng đúng các chữ số ban đầu. Hoán vị kế tiếp của toàn bộ chuỗi chưa chắc vẫn là số đối xứng.
>
> Một số đối xứng được quyết định bởi nửa đầu của nó. Ta tìm hoán vị kế tiếp của nửa đầu; nếu không tồn tại thì không có đáp án. Sau đó phản chiếu nửa đầu sang hậu tố để khôi phục tính đối xứng.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần tìm hoán vị kế tiếp của nửa đầu chuỗi, sau đó duyệt nửa đầu và gán các giá trị đối xứng cho nửa sau.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextPalindrome(self, num: str) -> str:
        def next_permutation(nums: List[str]) -> bool:
            n = len(nums) // 2
            i = n - 2
            while i >= 0 and nums[i] >= nums[i + 1]:
                i -= 1
            if i < 0:
                return False
            j = n - 1
            while j >= 0 and nums[j] <= nums[i]:
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]
            nums[i + 1 : n] = nums[i + 1 : n][::-1]
            return True

        nums = list(num)
        if not next_permutation(nums):
            return ""
        n = len(nums)
        for i in range(n // 2):
            nums[n - i - 1] = nums[i]
        return "".join(nums)
```

#### Java

```java
class Solution {
    public String nextPalindrome(String num) {
        char[] nums = num.toCharArray();
        if (!nextPermutation(nums)) {
            return "";
        }
        int n = nums.length;
        for (int i = 0; i < n / 2; ++i) {
            nums[n - 1 - i] = nums[i];
        }
        return String.valueOf(nums);
    }

    private boolean nextPermutation(char[] nums) {
        int n = nums.length / 2;
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            --i;
        }
        if (i < 0) {
            return false;
        }
        int j = n - 1;
        while (j >= 0 && nums[i] >= nums[j]) {
            --j;
        }
        swap(nums, i++, j);
        for (j = n - 1; i < j; ++i, --j) {
            swap(nums, i, j);
        }
        return true;
    }

    private void swap(char[] nums, int i, int j) {
        char t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string nextPalindrome(string num) {
        int n = num.size();
        string nums = num.substr(0, n / 2);
        if (!next_permutation(begin(nums), end(nums))) {
            return "";
        }
        for (int i = 0; i < n / 2; ++i) {
            num[i] = nums[i];
            num[n - i - 1] = nums[i];
        }
        return num;
    }
};
```

#### Go

```go
func nextPalindrome(num string) string {
	nums := []byte(num)
	n := len(nums)
	if !nextPermutation(nums) {
		return ""
	}
	for i := 0; i < n/2; i++ {
		nums[n-1-i] = nums[i]
	}
	return string(nums)
}

func nextPermutation(nums []byte) bool {
	n := len(nums) / 2
	i := n - 2
	for i >= 0 && nums[i] >= nums[i+1] {
		i--
	}
	if i < 0 {
		return false
	}
	j := n - 1
	for j >= 0 && nums[j] <= nums[i] {
		j--
	}
	nums[i], nums[j] = nums[j], nums[i]
	for i, j = i+1, n-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
	return true
}
```

#### TypeScript

```ts
function nextPalindrome(num: string): string {
    const nums = num.split('');
    const n = nums.length;
    if (!nextPermutation(nums)) {
        return '';
    }
    for (let i = 0; i < n >> 1; ++i) {
        nums[n - 1 - i] = nums[i];
    }
    return nums.join('');
}

function nextPermutation(nums: string[]): boolean {
    const n = nums.length >> 1;
    let i = n - 2;
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }
    if (i < 0) {
        return false;
    }
    let j = n - 1;
    while (j >= 0 && nums[i] >= nums[j]) {
        j--;
    }
    [nums[i], nums[j]] = [nums[j], nums[i]];
    for (i = i + 1, j = n - 1; i < j; ++i, --j) {
        [nums[i], nums[j]] = [nums[j], nums[i]];
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
