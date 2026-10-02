---
comments: true
difficulty: Hard
rating: 2050
source: Weekly Contest 158 Q4
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1224. Maximum Equal Frequency](https://leetcode.com/problems/maximum-equal-frequency)

[中文文档](/solution/1200-1299/1224.Maximum%20Equal%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>nums</code>. Hãy trả về độ dài lớn nhất của một tiền tố sao cho có thể xóa <strong>đúng một</strong> phần tử khỏi tiền tố đó để mọi số còn lại đều xuất hiện với số lần như nhau.</p>

<p>Nếu sau khi xóa một phần tử mà không còn phần tử nào, ta vẫn xem như mọi số xuất hiện số lần bằng nhau (0).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,1,1,5,3,3,5]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Với mảng con [2,2,1,1,5,3,3] có độ dài 7, nếu xóa nums[4] = 5, ta được [2,2,1,1,3,3], trong đó mỗi số xuất hiện đúng hai lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,2,2,2,3,3,3,4,4,4,5]
<strong>Đầu ra:</strong> 13
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc hash table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tiền tố dài nhất mà khi xóa một phần tử, tần suất của các số còn lại bằng nhau. Vì $n \le 10^5$, không thể đếm lại tần suất cho từng tiền tố.
>
> Duy trì tần suất từng giá trị $cnt$, số lượng giá trị có mỗi tần suất $ccnt$ và tần suất lớn nhất $mx$. Một tiền tố hợp lệ khi mọi tần suất đều bằng $1$; hoặc chỉ có $mx$ và $mx-1$, trong đó chỉ một giá trị có tần suất $mx$; hoặc tất cả đều có tần suất $mx$ ngoại trừ một giá trị chỉ xuất hiện một lần.
>
> Ta cập nhật hai bảng từ trái sang phải, rồi dùng $ccnt$ và $mx$ để kiểm tra ba trường hợp trên trong $O(1)$, đồng thời ghi nhận chỉ số lớn nhất của tiền tố hợp lệ.

<!-- thinking:end -->

Dùng $cnt$ để ghi nhận số lần mỗi phần tử $v$ xuất hiện trong $nums$, và $ccnt$ để ghi nhận có bao nhiêu giá trị cùng xuất hiện một số lần nhất định. Tần suất lớn nhất được ký hiệu là $mx$.

Khi duyệt $nums$:

- Nếu tần suất lớn nhất $mx=1$, nghĩa là mỗi số trong tiền tố hiện tại xuất hiện 1 lần. Xóa số nào cũng được, các số còn lại vẫn có cùng tần suất.
- Nếu các số chỉ xuất hiện $mx$ hoặc $mx-1$ lần, và chỉ có một số xuất hiện $mx$ lần, ta có thể xóa một lần xuất hiện của số đó. Khi ấy, mọi số còn lại đều có tần suất $mx-1$, thỏa điều kiện.
- Nếu tất cả số trừ một số đều xuất hiện $mx$ lần, ta có thể xóa số chỉ xuất hiện một lần đó. Khi ấy, mọi số còn lại đều có tần suất $mx$, thỏa điều kiện.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEqualFreq(self, nums: List[int]) -> int:
        cnt = Counter()
        ccnt = Counter()
        ans = mx = 0
        for i, v in enumerate(nums, 1):
            if v in cnt:
                ccnt[cnt[v]] -= 1
            cnt[v] += 1
            mx = max(mx, cnt[v])
            ccnt[cnt[v]] += 1
            if mx == 1:
                ans = i
            elif ccnt[mx] * mx + ccnt[mx - 1] * (mx - 1) == i and ccnt[mx] == 1:
                ans = i
            elif ccnt[mx] * mx + 1 == i and ccnt[1] == 1:
                ans = i
        return ans
```

#### Java

```java
class Solution {
    private static int[] cnt = new int[100010];
    private static int[] ccnt = new int[100010];

    public int maxEqualFreq(int[] nums) {
        Arrays.fill(cnt, 0);
        Arrays.fill(ccnt, 0);
        int ans = 0;
        int mx = 0;
        for (int i = 1; i <= nums.length; ++i) {
            int v = nums[i - 1];
            if (cnt[v] > 0) {
                --ccnt[cnt[v]];
            }
            ++cnt[v];
            mx = Math.max(mx, cnt[v]);
            ++ccnt[cnt[v]];
            if (mx == 1) {
                ans = i;
            } else if (ccnt[mx] * mx + ccnt[mx - 1] * (mx - 1) == i && ccnt[mx] == 1) {
                ans = i;
            } else if (ccnt[mx] * mx + 1 == i && ccnt[1] == 1) {
                ans = i;
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
    int maxEqualFreq(vector<int>& nums) {
        unordered_map<int, int> cnt;
        unordered_map<int, int> ccnt;
        int ans = 0, mx = 0;
        for (int i = 1; i <= nums.size(); ++i) {
            int v = nums[i - 1];
            if (cnt[v]) --ccnt[cnt[v]];
            ++cnt[v];
            mx = max(mx, cnt[v]);
            ++ccnt[cnt[v]];
            if (mx == 1)
                ans = i;
            else if (ccnt[mx] * mx + ccnt[mx - 1] * (mx - 1) == i && ccnt[mx] == 1)
                ans = i;
            else if (ccnt[mx] * mx + 1 == i && ccnt[1] == 1)
                ans = i;
        }
        return ans;
    }
};
```

#### Go

```go
func maxEqualFreq(nums []int) int {
	cnt := map[int]int{}
	ccnt := map[int]int{}
	ans, mx := 0, 0
	for i, v := range nums {
		i++
		if cnt[v] > 0 {
			ccnt[cnt[v]]--
		}
		cnt[v]++
		mx = max(mx, cnt[v])
		ccnt[cnt[v]]++
		if mx == 1 {
			ans = i
		} else if ccnt[mx]*mx+ccnt[mx-1]*(mx-1) == i && ccnt[mx] == 1 {
			ans = i
		} else if ccnt[mx]*mx+1 == i && ccnt[1] == 1 {
			ans = i
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxEqualFreq(nums: number[]): number {
    const n = nums.length;
    const map = new Map();
    for (const num of nums) {
        map.set(num, (map.get(num) ?? 0) + 1);
    }

    for (let i = n - 1; i > 0; i--) {
        for (const k of map.keys()) {
            map.set(k, map.get(k) - 1);
            let num = 0;
            for (const v of map.values()) {
                if (v !== 0) {
                    num = v;
                    break;
                }
            }
            let isOk = true;
            let sum = 1;
            for (const v of map.values()) {
                if (v !== 0 && v !== num) {
                    isOk = false;
                    break;
                }
                sum += v;
            }
            if (isOk) {
                return sum;
            }
            map.set(k, map.get(k) + 1);
        }
        map.set(nums[i], map.get(nums[i]) - 1);
    }
    return 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
