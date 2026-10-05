---
comments: true
difficulty: Easy
rating: 1201
source: Weekly Contest 503 Q1
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [3940. Limit Occurrences in Sorted Array](https://leetcode.com/problems/limit-occurrences-in-sorted-array)

[中文文档](/solution/3900-3999/3940.Limit%20Occurrences%20in%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>đã sắp xếp</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trả về một mảng sao cho mỗi phần tử <strong>phân biệt</strong> xuất hiện <strong>nhiều nhất</strong> <code>k</code> lần, đồng thời giữ nguyên thứ tự tương đối của các phần tử trong <code>nums</code>.</p>

<p>Lưu ý: Nếu một phần tử phân biệt xuất hiện <strong>ít nhất</strong> <code>k</code> lần, thì nó phải xuất hiện <strong>chính xác</strong> <code>k</code> lần trong mảng kết quả.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,2,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,2,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mỗi phần tử có thể xuất hiện nhiều nhất 2 lần.</p>

<ul>
	<li>Phần tử 1 xuất hiện 3 lần, nên chỉ giữ lại 2 lần xuất hiện.</li>
	<li>Phần tử 2 xuất hiện 2 lần, nên giữ lại cả hai lần.</li>
	<li>Phần tử 3 xuất hiện 1 lần, nên được giữ lại.</li>
</ul>

<p>Do đó, mảng kết quả là <code>[1, 1, 2, 2, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả phần tử đều phân biệt và đã xuất hiện nhiều nhất một lần, nên mảng không thay đổi.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>nums</code> được sắp xếp theo thứ tự không giảm.</li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Bạn có thể giải bài này ngay trên mảng với bộ nhớ bổ sung O(1) không?</li>
	<li>Lưu ý rằng bộ nhớ dùng để trả về hoặc thay đổi kích thước kết quả không được tính vào độ phức tạp bộ nhớ nêu trên, vì một số ngôn ngữ không hỗ trợ thay đổi kích thước ngay trên mảng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã được sắp xếp, nên các giá trị bằng nhau tạo thành từng đoạn liên tiếp. Mỗi đoạn cần được cắt còn độ dài $k$ và ghi lại vào một tiền tố.
>
> Con trỏ chậm $l$ đánh dấu vị trí ghi tiếp theo, còn con trỏ nhanh $r$ dùng để duyệt mảng. Khi giá trị thay đổi, bộ đếm được đặt lại thành $1$; nếu không, bộ đếm tăng lên. Chỉ ghi phần tử khi bộ đếm không vượt quá $k$.
>
> $n\le 100$, nên chỉ cần một lần duyệt tuyến tính.

<!-- thinking:end -->

Ta định nghĩa hai con trỏ $l$ và $r$, trong đó $l$ là vị trí ghi còn $r$ là vị trí đang đọc. Ta cũng dùng bộ đếm $cnt$ để ghi nhận số lần giá trị hiện tại đã xuất hiện. Ban đầu, cả $l$ và $cnt$ đều được đặt bằng 1.

Sau đó, ta duyệt mảng bắt đầu từ $r = 1$:

1. Nếu $nums[r] \ne nums[r - 1]$, ta gặp một giá trị mới, nên đặt lại $cnt$ thành 1.
2. Nếu $nums[r] = nums[r - 1]$, đây là một phần tử trùng lặp, nên tăng $cnt$ lên 1.

Nếu $cnt \le k$, giới hạn số lần xuất hiện chưa bị vượt qua, nên ta giữ phần tử này bằng cách ghi $nums[r]$ vào $nums[l]$, rồi tăng $l$ lên một.

Cuối cùng, trả về $l$ phần tử đầu tiên, tức là $nums[:l]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp bộ nhớ là $O(1)$ vì chỉ sử dụng bộ nhớ bổ sung hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def limitOccurrences(self, nums: list[int], k: int) -> list[int]:
        n = len(nums)
        cnt = l = 1
        for r in range(1, n):
            if nums[r] != nums[r - 1]:
                cnt = 1
            else:
                cnt += 1
            if cnt <= k:
                nums[l] = nums[r]
                l += 1
        return nums[:l]
```

#### Java

```java
class Solution {
    public int[] limitOccurrences(int[] nums, int k) {
        int n = nums.length;
        int cnt = 1, l = 1;

        for (int r = 1; r < n; r++) {
            if (nums[r] != nums[r - 1]) {
                cnt = 1;
            } else {
                cnt++;
            }

            if (cnt <= k) {
                nums[l] = nums[r];
                l++;
            }
        }

        return Arrays.copyOf(nums, l);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> limitOccurrences(vector<int>& nums, int k) {
        int n = nums.size();
        int cnt = 1, l = 1;

        for (int r = 1; r < n; r++) {
            if (nums[r] != nums[r - 1]) {
                cnt = 1;
            } else {
                cnt++;
            }

            if (cnt <= k) {
                nums[l] = nums[r];
                l++;
            }
        }

        nums.resize(l);
        return nums;
    }
};
```

#### Go

```go
func limitOccurrences(nums []int, k int) []int {
	n := len(nums)
	cnt, l := 1, 1

	for r := 1; r < n; r++ {
		if nums[r] != nums[r-1] {
			cnt = 1
		} else {
			cnt++
		}

		if cnt <= k {
			nums[l] = nums[r]
			l++
		}
	}

	return nums[:l]
}
```

#### TypeScript

```ts
function limitOccurrences(nums: number[], k: number): number[] {
    const n = nums.length;
    let cnt = 1;
    let l = 1;

    for (let r = 1; r < n; r++) {
        if (nums[r] !== nums[r - 1]) {
            cnt = 1;
        } else {
            cnt++;
        }

        if (cnt <= k) {
            nums[l] = nums[r];
            l++;
        }
    }

    return nums.slice(0, l);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
