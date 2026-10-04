---
comments: true
difficulty: Medium
rating: 1700
source: Weekly Contest 375 Q3
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [2962. Count Subarrays Where Max Element Appears at Least K Times](https://leetcode.com/problems/count-subarrays-where-max-element-appears-at-least-k-times)

[中文文档](/solution/2900-2999/2962.Count%20Subarrays%20Where%20Max%20Element%20Appears%20at%20Least%20K%20Times/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Trả về <em>số lượng mảng con trong đó phần tử <strong>lớn nhất</strong> của </em><code>nums</code><em> xuất hiện <strong>ít nhất</strong> </em><code>k</code><em> lần trong mảng con đó.</em></p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,3,3], k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các mảng con chứa phần tử 3 ít nhất 2 lần là: [1,3,2,3], [1,3,2,3,3], [3,2,3], [3,2,3,3], [2,3,3] và [3,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,2,1], k = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con nào chứa phần tử 4 ít nhất 3 lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con phải chứa giá trị lớn nhất toàn cục ít nhất $k$ lần. Vì $n \le 10^5$, không thể liệt kê tất cả mảng con. Với một điểm bắt đầu cố định, điểm kết thúc sớm nhất chứa đủ $k$ bản sao của $mx$ có tính đơn điệu, và mọi điểm kết thúc muộn hơn đều vẫn hợp lệ.
>
> Hai con trỏ duy trì $cnt$. Sau mỗi bước dịch trái, ta tăng $j$ nếu cần, cộng $n-j+1$, rồi loại bỏ $mx$ đang rời khỏi cửa sổ.

<!-- thinking:end -->

Gọi giá trị lớn nhất trong mảng là $mx$.

Ta sử dụng hai con trỏ $i$ và $j$ để duy trì một cửa sổ trượt sao cho trong mảng con $[i, j)$ có $k$ phần tử bằng $mx$. Nếu cố định điểm đầu trái $i$, thì mọi điểm đầu phải lớn hơn hoặc bằng $j-1$ đều thỏa mãn điều kiện, có tổng cộng $n - (j - 1)$ điểm.

Do đó, ta duyệt qua điểm đầu trái $i$, dùng con trỏ $j$ để duy trì điểm đầu phải, đồng thời dùng biến $cnt$ để ghi nhận số phần tử bằng $mx$ trong cửa sổ hiện tại. Khi $cnt$ lớn hơn hoặc bằng $k$, ta đã tìm được một mảng con thỏa mãn điều kiện và tăng đáp án thêm $n - (j - 1)$. Sau đó, ta cập nhật $cnt$ và tiếp tục duyệt điểm đầu trái tiếp theo.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        mx = max(nums)
        n = len(nums)
        ans = cnt = j = 0
        for x in nums:
            while j < n and cnt < k:
                cnt += nums[j] == mx
                j += 1
            if cnt < k:
                break
            ans += n - j + 1
            cnt -= x == mx
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums, int k) {
        int mx = Arrays.stream(nums).max().getAsInt();
        int n = nums.length;
        long ans = 0;
        int cnt = 0, j = 0;
        for (int x : nums) {
            while (j < n && cnt < k) {
                cnt += nums[j++] == mx ? 1 : 0;
            }
            if (cnt < k) {
                break;
            }
            ans += n - j + 1;
            cnt -= x == mx ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, int k) {
        int mx = *max_element(nums.begin(), nums.end());
        int n = nums.size();
        long long ans = 0;
        int cnt = 0, j = 0;
        for (int x : nums) {
            while (j < n && cnt < k) {
                cnt += nums[j++] == mx;
            }
            if (cnt < k) {
                break;
            }
            ans += n - j + 1;
            cnt -= x == mx;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int) (ans int64) {
	mx := slices.Max(nums)
	n := len(nums)
	cnt, j := 0, 0
	for _, x := range nums {
		for ; j < n && cnt < k; j++ {
			if nums[j] == mx {
				cnt++
			}
		}
		if cnt < k {
			break
		}
		ans += int64(n - j + 1)
		if x == mx {
			cnt--
		}
	}
	return
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number): number {
    const mx = Math.max(...nums);
    const n = nums.length;
    let [cnt, j] = [0, 0];
    let ans = 0;
    for (const x of nums) {
        for (; j < n && cnt < k; ++j) {
            cnt += nums[j] === mx ? 1 : 0;
        }
        if (cnt < k) {
            break;
        }
        ans += n - j + 1;
        cnt -= x === mx ? 1 : 0;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_subarrays(nums: Vec<i32>, k: i32) -> i64 {
        let mx = *nums.iter().max().unwrap();
        let n = nums.len();
        let mut ans = 0i64;
        let mut cnt = 0;
        let mut j = 0;

        for &x in &nums {
            while j < n && cnt < k {
                if nums[j] == mx {
                    cnt += 1;
                }
                j += 1;
            }

            if cnt < k {
                break;
            }

            ans += (n - j + 1) as i64;

            if x == mx {
                cnt -= 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
