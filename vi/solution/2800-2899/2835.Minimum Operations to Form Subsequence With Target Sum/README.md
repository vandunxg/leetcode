---
comments: true
difficulty: Hard
rating: 2207
source: Weekly Contest 360 Q3
tags:
    - Greedy
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2835. Minimum Operations to Form Subsequence With Target Sum](https://leetcode.com/problems/minimum-operations-to-form-subsequence-with-target-sum)

[中文文档](/solution/2800-2899/2835.Minimum%20Operations%20to%20Form%20Subsequence%20With%20Target%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, trong đó các phần tử là <strong>lũy thừa không âm</strong> của <code>2</code>, và một số nguyên <code>target</code>.</p>

<p>Trong một thao tác, bạn phải thực hiện các thay đổi sau trên mảng:</p>

<ul>
	<li>Chọn một phần tử bất kỳ <code>nums[i]</code> sao cho <code>nums[i] &gt; 1</code>.</li>
	<li>Xóa <code>nums[i]</code> khỏi mảng.</li>
	<li>Thêm <strong>hai</strong> lần xuất hiện của <code>nums[i] / 2</code> vào <strong>cuối</strong> mảng <code>nums</code>.</li>
</ul>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để </em><code>nums</code><em> chứa một <strong>dãy con</strong> có tổng các phần tử bằng</em> <code>target</code>. Nếu không thể tạo ra dãy con như vậy, trả về <code>-1</code>.</p>

<p><strong>Dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,8], target = 7
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong thao tác đầu tiên, ta chọn phần tử nums[2]. Mảng trở thành nums = [1,2,4,4].
Lúc này, nums chứa dãy con [1,2,4] có tổng bằng 7.
Có thể chứng minh rằng không có chuỗi thao tác nào ngắn hơn tạo ra một dãy con có tổng bằng 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,32,1,2], target = 12
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong thao tác đầu tiên, ta chọn phần tử nums[1]. Mảng trở thành nums = [1,1,2,16,16].
Trong thao tác thứ hai, ta chọn phần tử nums[3]. Mảng trở thành nums = [1,1,2,16,8,8]
Lúc này, nums chứa dãy con [1,1,2,8] có tổng bằng 12.
Có thể chứng minh rằng không có chuỗi thao tác nào ngắn hơn tạo ra một dãy con có tổng bằng 12.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,32,1], target = 35
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Có thể chứng minh rằng không có chuỗi thao tác nào tạo ra một dãy con có tổng bằng 35.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2<sup>30</sup></code></li>
	<li><code>nums</code> chỉ gồm các lũy thừa không âm của 2.</li>
	<li><code>1 &lt;= target &lt; 2<sup>31</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác tách một lũy thừa của 2 thành hai số nhỏ hơn bằng nhau và giữ nguyên tổng. Nếu tổng nhỏ hơn $target$ thì không có đáp án. Sau khi đếm các bit, ta thỏa mãn từng bit $1$ của $target$ từ thấp lên cao: khi thiếu ở bit hiện tại, tách một bit cao hơn và tính một thao tác cho mỗi lần tách.

<!-- thinking:end -->

Quan sát thao tác trong đề bài, ta thấy mỗi thao tác thực chất tách một số lớn hơn $1$ thành hai số bằng nhau, nghĩa là tổng các phần tử trong mảng không thay đổi sau thao tác. Do đó, nếu tổng các phần tử trong mảng $s$ nhỏ hơn $target$, không thể tạo ra một dãy con có tổng bằng $target$ bằng thao tác được mô tả trong đề bài, và ta có thể trực tiếp trả về $-1$. Ngược lại, ta chắc chắn có thể làm cho tổng của một dãy con nào đó trong mảng bằng $target$ thông qua các thao tác tách.

Ngoài ra, thao tác tách thực chất sẽ đặt bit cao của số về $0$ và cộng $2$ vào bit thấp hơn. Vì vậy, trước tiên ta dùng một mảng có độ dài $32$ để ghi nhận số lần bit $1$ xuất hiện ở mỗi vị trí trong biểu diễn nhị phân của tất cả các phần tử trong mảng $nums$.

Tiếp theo, bắt đầu từ bit thấp nhất của $target$, với bit thứ $i$ của $target$, nếu số lượng bit hiện tại bằng $0$, ta bỏ qua trực tiếp, tức là $i = i + 1$. Nếu số lượng bit hiện tại bằng $1$, ta cần tìm chỉ số nhỏ nhất $j$ (với $j \ge i$) trong mảng $cnt$ sao cho $cnt[j] > 0$, rồi tách số $1$ ở bit này xuống bit $i$, tức là giảm $cnt[j]$ đi $1$, đặt mỗi bit từ $i$ đến $j-1$ trong $cnt$ thành $1$, và số thao tác là $j-i$. Tiếp theo, ta gán $j = i$, rồi $i = i + 1$. Lặp lại thao tác trên cho đến khi $i$ vượt quá phạm vi chỉ số của mảng $cnt$, rồi trả về số thao tác tại thời điểm đó.

Lưu ý rằng nếu $j < i$, thực chất hai bit thấp hơn $1$ có thể được gộp thành một bit cao hơn bằng $1$. Do đó, nếu $j < i$, ta cộng $\frac{cnt[j]}{2}$ vào $cnt[j+1]$, lấy $cnt[j]$ modulo $2$, rồi gán $j = j + 1$ và tiếp tục thao tác trên.

Độ phức tạp thời gian là $O(n \times \log M)$, độ phức tạp không gian là $O(\log M)$. Trong đó, $n$ là độ dài của mảng $nums$ và $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], target: int) -> int:
        s = sum(nums)
        if s < target:
            return -1
        cnt = [0] * 32
        for x in nums:
            for i in range(32):
                if x >> i & 1:
                    cnt[i] += 1
        i = j = 0
        ans = 0
        while 1:
            while i < 32 and (target >> i & 1) == 0:
                i += 1
            if i == 32:
                break
            while j < i:
                cnt[j + 1] += cnt[j] // 2
                cnt[j] %= 2
                j += 1
            while cnt[j] == 0:
                cnt[j] = 1
                j += 1
            ans += j - i
            cnt[j] -= 1
            j = i
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(List<Integer> nums, int target) {
        long s = 0;
        int[] cnt = new int[32];
        for (int x : nums) {
            s += x;
            for (int i = 0; i < 32; ++i) {
                if ((x >> i & 1) == 1) {
                    ++cnt[i];
                }
            }
        }
        if (s < target) {
            return -1;
        }
        int i = 0, j = 0;
        int ans = 0;
        while (true) {
            while (i < 32 && (target >> i & 1) == 0) {
                ++i;
            }
            if (i == 32) {
                return ans;
            }
            while (j < i) {
                cnt[j + 1] += cnt[j] / 2;
                cnt[j] %= 2;
                ++j;
            }
            while (cnt[j] == 0) {
                cnt[j] = 1;
                ++j;
            }
            ans += j - i;
            --cnt[j];
            j = i;
            ++i;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int target) {
        long long s = 0;
        int cnt[32]{};
        for (int x : nums) {
            s += x;
            for (int i = 0; i < 32; ++i) {
                if (x >> i & 1) {
                    ++cnt[i];
                }
            }
        }
        if (s < target) {
            return -1;
        }
        int i = 0, j = 0;
        int ans = 0;
        while (1) {
            while (i < 32 && (target >> i & 1) == 0) {
                ++i;
            }
            if (i == 32) {
                return ans;
            }
            while (j < i) {
                cnt[j + 1] += cnt[j] / 2;
                cnt[j] %= 2;
                ++j;
            }
            while (cnt[j] == 0) {
                cnt[j] = 1;
                ++j;
            }
            ans += j - i;
            --cnt[j];
            j = i;
            ++i;
        }
    }
};
```

#### Go

```go
func minOperations(nums []int, target int) (ans int) {
	s := 0
	cnt := [32]int{}
	for _, x := range nums {
		s += x
		for i := 0; i < 32; i++ {
			if x>>i&1 > 0 {
				cnt[i]++
			}
		}
	}
	if s < target {
		return -1
	}
	var i, j int
	for {
		for i < 32 && target>>i&1 == 0 {
			i++
		}
		if i == 32 {
			return
		}
		for j < i {
			cnt[j+1] += cnt[j] >> 1
			cnt[j] %= 2
			j++
		}
		for cnt[j] == 0 {
			cnt[j] = 1
			j++
		}
		ans += j - i
		cnt[j]--
		j = i
		i++
	}
}
```

#### TypeScript

```ts
function minOperations(nums: number[], target: number): number {
    let s = 0;
    const cnt: number[] = Array(32).fill(0);
    for (const x of nums) {
        s += x;
        for (let i = 0; i < 32; ++i) {
            if ((x >> i) & 1) {
                ++cnt[i];
            }
        }
    }
    if (s < target) {
        return -1;
    }
    let [ans, i, j] = [0, 0, 0];
    while (1) {
        while (i < 32 && ((target >> i) & 1) === 0) {
            ++i;
        }
        if (i === 32) {
            return ans;
        }
        while (j < i) {
            cnt[j + 1] += cnt[j] >> 1;
            cnt[j] %= 2;
            ++j;
        }
        while (cnt[j] == 0) {
            cnt[j] = 1;
            j++;
        }
        ans += j - i;
        cnt[j]--;
        j = i;
        i++;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
