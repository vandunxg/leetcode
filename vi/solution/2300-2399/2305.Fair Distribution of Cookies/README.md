---
comments: true
difficulty: Medium
rating: 1886
source: Weekly Contest 297 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [2305. Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies)

[中文文档](/solution/2300-2399/2305.Fair%20Distribution%20of%20Cookies/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>cookies</code>, trong đó <code>cookies[i]</code> là số lượng bánh quy trong túi thứ <code>i<sup>th</sup></code>. Đồng thời, cho một số nguyên <code>k</code> biểu thị số trẻ em sẽ nhận <strong>tất cả</strong> các túi bánh quy. Tất cả bánh quy trong cùng một túi phải được đưa cho cùng một trẻ và không được chia nhỏ.</p>

<p><strong>Độ không công bằng</strong> của một cách phân phối được định nghĩa là <strong>tổng</strong> số bánh quy <strong>lớn nhất</strong> mà một trẻ nhận được trong cách phân phối đó.</p>

<p>Trả về <em><strong>độ không công bằng nhỏ nhất</strong> trong tất cả các cách phân phối</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cookies = [8,15,10,20,8], k = 2
<strong>Đầu ra:</strong> 31
<strong>Giải thích:</strong> Một cách phân phối tối ưu là [8,15,8] và [10,20]
- Trẻ thứ <sup>1</sup> nhận [8,15,8], có tổng là 8 + 15 + 8 = 31 chiếc bánh quy.
- Trẻ thứ <sup>2</sup> nhận [10,20], có tổng là 10 + 20 = 30 chiếc bánh quy.
Độ không công bằng của cách phân phối là max(31,30) = 31.
Có thể chứng minh rằng không có cách phân phối nào có độ không công bằng nhỏ hơn 31.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cookies = [6,1,3,2,2,4,1,2], k = 3
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Một cách phân phối tối ưu là [6,1], [3,2,2] và [4,1,2]
- Trẻ thứ <sup>1</sup> nhận [6,1], có tổng là 6 + 1 = 7 chiếc bánh quy.
- Trẻ thứ <sup>2</sup> nhận [3,2,2], có tổng là 3 + 2 + 2 = 7 chiếc bánh quy.
- Trẻ thứ <sup>3</sup> nhận [4,1,2], có tổng là 4 + 1 + 2 = 7 chiếc bánh quy.
Độ không công bằng của cách phân phối là max(7,7,7) = 7.
Có thể chứng minh rằng không có cách phân phối nào có độ không công bằng nhỏ hơn 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= cookies.length &lt;= 8</code></li>
	<li><code>1 &lt;= cookies[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= k &lt;= cookies.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quay lui + Cắt tỉa

<!-- thinking:start -->

> **Tư duy**
>
> Ta phân phối $n \le 8$ túi bánh quy cho $k \le 8$ trẻ em và tối thiểu hóa tải lớn nhất. Nếu không có ràng buộc, số trạng thái phân phối là $k^n$, quá lớn khi các giá trị đạt giới hạn.
>
> Độ không công bằng không phụ thuộc vào thứ tự các túi, nhưng đặt các túi lớn trước sẽ giúp cắt tỉa sớm hơn. Sắp xếp các túi theo thứ tự giảm dần, rồi quay lui: bỏ qua một trẻ nếu thêm túi hiện tại đã không tốt hơn đáp án đã biết; coi các tải bằng nhau của những trẻ liền kề là đối xứng. Sau khi đã phân phối mỗi túi, cập nhật đáp án bằng tải lớn nhất hiện tại.

<!-- thinking:end -->

Trước hết, ta sắp xếp mảng $cookies$ theo thứ tự giảm dần (để giảm số lần tìm kiếm), sau đó tạo một mảng $cnt$ có độ dài $k$ để lưu số bánh quy mỗi trẻ nhận được. Ngoài ra, ta sử dụng biến $ans$ để duy trì độ không công bằng nhỏ nhất hiện tại, khởi tạo bằng một giá trị rất lớn.

Tiếp theo, ta bắt đầu từ túi bánh quy đầu tiên. Với túi hiện tại $i$, ta lần lượt xét từng trẻ $j$. Nếu phân phối số bánh quy $cookies[i]$ trong túi hiện tại cho trẻ $j$ khiến độ không công bằng lớn hơn hoặc bằng $ans$, hoặc số bánh quy mà trẻ hiện tại đã có bằng với trẻ trước đó, thì ta không cần xét việc phân phối túi hiện tại cho trẻ $j$ nữa mà bỏ qua (cắt tỉa). Ngược lại, ta phân phối số bánh quy $cookies[i]$ trong túi hiện tại cho trẻ $j$, rồi tiếp tục xét túi tiếp theo. Khi đã xét hết các túi, ta cập nhật giá trị của $ans$, sau đó quay lui về túi trước đó và tiếp tục lần lượt xét việc phân phối túi hiện tại cho trẻ nào.

Cuối cùng, ta trả về $ans$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distributeCookies(self, cookies: List[int], k: int) -> int:
        def dfs(i):
            if i >= len(cookies):
                nonlocal ans
                ans = max(cnt)
                return
            for j in range(k):
                if cnt[j] + cookies[i] >= ans or (j and cnt[j] == cnt[j - 1]):
                    continue
                cnt[j] += cookies[i]
                dfs(i + 1)
                cnt[j] -= cookies[i]

        ans = inf
        cnt = [0] * k
        cookies.sort(reverse=True)
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private int[] cookies;
    private int[] cnt;
    private int k;
    private int n;
    private int ans = 1 << 30;

    public int distributeCookies(int[] cookies, int k) {
        n = cookies.length;
        cnt = new int[k];
        // 升序排列
        Arrays.sort(cookies);
        this.cookies = cookies;
        this.k = k;
        // 这里搜索顺序是 n-1, n-2,...0
        dfs(n - 1);
        return ans;
    }

    private void dfs(int i) {
        if (i < 0) {
            // ans = Arrays.stream(cnt).max().getAsInt();
            ans = 0;
            for (int v : cnt) {
                ans = Math.max(ans, v);
            }
            return;
        }
        for (int j = 0; j < k; ++j) {
            if (cnt[j] + cookies[i] >= ans || (j > 0 && cnt[j] == cnt[j - 1])) {
                continue;
            }
            cnt[j] += cookies[i];
            dfs(i - 1);
            cnt[j] -= cookies[i];
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distributeCookies(vector<int>& cookies, int k) {
        sort(cookies.rbegin(), cookies.rend());
        int cnt[k];
        memset(cnt, 0, sizeof cnt);
        int n = cookies.size();
        int ans = 1 << 30;
        function<void(int)> dfs = [&](int i) {
            if (i >= n) {
                ans = *max_element(cnt, cnt + k);
                return;
            }
            for (int j = 0; j < k; ++j) {
                if (cnt[j] + cookies[i] >= ans || (j && cnt[j] == cnt[j - 1])) {
                    continue;
                }
                cnt[j] += cookies[i];
                dfs(i + 1);
                cnt[j] -= cookies[i];
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func distributeCookies(cookies []int, k int) int {
	sort.Sort(sort.Reverse(sort.IntSlice(cookies)))
	cnt := make([]int, k)
	ans := 1 << 30
	var dfs func(int)
	dfs = func(i int) {
		if i >= len(cookies) {
			ans = slices.Max(cnt)
			return
		}
		for j := 0; j < k; j++ {
			if cnt[j]+cookies[i] >= ans || (j > 0 && cnt[j] == cnt[j-1]) {
				continue
			}
			cnt[j] += cookies[i]
			dfs(i + 1)
			cnt[j] -= cookies[i]
		}
	}
	dfs(0)
	return ans
}
```

#### TypeScript

```ts
function distributeCookies(cookies: number[], k: number): number {
    const cnt = new Array(k).fill(0);
    let ans = 1 << 30;
    const dfs = (i: number) => {
        if (i >= cookies.length) {
            ans = Math.max(...cnt);
            return;
        }
        for (let j = 0; j < k; ++j) {
            if (cnt[j] + cookies[i] >= ans || (j && cnt[j] == cnt[j - 1])) {
                continue;
            }
            cnt[j] += cookies[i];
            dfs(i + 1);
            cnt[j] -= cookies[i];
        }
    };
    dfs(0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
