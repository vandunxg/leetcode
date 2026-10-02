---
comments: true
difficulty: Hard
rating: 2284
source: Weekly Contest 223 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [1723. Find Minimum Time to Finish All Jobs](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs)

[中文文档](/solution/1700-1799/1723.Find%20Minimum%20Time%20to%20Finish%20All%20Jobs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>jobs</code>, trong đó <code>jobs[i]</code> là thời gian cần để hoàn thành công việc thứ <code>i<sup>th</sup></code>.</p>

<p>Có <code>k</code> worker để ta phân công các công việc. Mỗi công việc phải được giao cho <strong>chính xác</strong> một worker. <strong>Thời gian làm việc</strong> của một worker là tổng thời gian hoàn thành tất cả công việc được giao. Mục tiêu là tìm cách phân công tối ưu sao cho <strong>thời gian làm việc lớn nhất</strong> của mọi worker là <strong>nhỏ nhất</strong>.</p>

<p><em>Trả về <strong>nhỏ nhất</strong> của <strong>thời gian làm việc lớn nhất</strong> có thể đạt được trong mọi cách phân công. </em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> jobs = [3,2,3], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Nếu giao mỗi người một công việc, thời gian lớn nhất là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> jobs = [1,2,4,7,8], k = 2
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Phân công các công việc như sau:
Worker 1: 1, 2, 8 (thời gian làm việc = 1 + 2 + 8 = 11)
Worker 2: 4, 7 (thời gian làm việc = 4 + 7 = 11)
Thời gian làm việc lớn nhất là 11.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= jobs.length &lt;= 12</code></li>
	<li><code>1 &lt;= jobs[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phân công công việc cho $k$ worker và tối thiểu hóa tải lớn nhất. Số lượng công việc đủ nhỏ để tìm kiếm, nhưng cách thử tất cả $k^n$ khả năng là quá lớn.
>
> Bỏ một nhánh ngay khi tải lớn nhất hiện tại không tốt hơn đáp án đã ghi nhận. Phân công các công việc dài trước giúp cắt tỉa sớm hơn.
>
> Sắp xếp $jobs$ giảm dần rồi DFS vào từng worker và hoàn tác sau đó. Nếu một worker vẫn đang trống, bỏ qua các worker trống phía sau để loại bỏ các cách phân công đối xứng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTimeRequired(self, jobs: List[int], k: int) -> int:
        def dfs(i):
            nonlocal ans
            if i == len(jobs):
                ans = min(ans, max(cnt))
                return
            for j in range(k):
                if cnt[j] + jobs[i] >= ans:
                    continue
                cnt[j] += jobs[i]
                dfs(i + 1)
                cnt[j] -= jobs[i]
                if cnt[j] == 0:
                    break

        cnt = [0] * k
        jobs.sort(reverse=True)
        ans = inf
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private int[] cnt;
    private int ans;
    private int[] jobs;
    private int k;

    public int minimumTimeRequired(int[] jobs, int k) {
        this.k = k;
        Arrays.sort(jobs);
        for (int i = 0, j = jobs.length - 1; i < j; ++i, --j) {
            int t = jobs[i];
            jobs[i] = jobs[j];
            jobs[j] = t;
        }
        this.jobs = jobs;
        cnt = new int[k];
        ans = 0x3f3f3f3f;
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i == jobs.length) {
            int mx = 0;
            for (int v : cnt) {
                mx = Math.max(mx, v);
            }
            ans = Math.min(ans, mx);
            return;
        }
        for (int j = 0; j < k; ++j) {
            if (cnt[j] + jobs[i] >= ans) {
                continue;
            }
            cnt[j] += jobs[i];
            dfs(i + 1);
            cnt[j] -= jobs[i];
            if (cnt[j] == 0) {
                break;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int ans;

    int minimumTimeRequired(vector<int>& jobs, int k) {
        vector<int> cnt(k);
        ans = 0x3f3f3f3f;
        sort(jobs.begin(), jobs.end(), greater<int>());
        dfs(0, k, jobs, cnt);
        return ans;
    }

    void dfs(int i, int k, vector<int>& jobs, vector<int>& cnt) {
        if (i == jobs.size()) {
            ans = min(ans, *max_element(cnt.begin(), cnt.end()));
            return;
        }
        for (int j = 0; j < k; ++j) {
            if (cnt[j] + jobs[i] >= ans) continue;
            cnt[j] += jobs[i];
            dfs(i + 1, k, jobs, cnt);
            cnt[j] -= jobs[i];
            if (cnt[j] == 0) break;
        }
    }
};
```

#### Go

```go
func minimumTimeRequired(jobs []int, k int) int {
	cnt := make([]int, k)
	ans := 0x3f3f3f3f
	sort.Slice(jobs, func(i, j int) bool {
		return jobs[i] > jobs[j]
	})
	var dfs func(int)
	dfs = func(i int) {
		if i == len(jobs) {
			mx := slices.Max(cnt)
			ans = min(ans, mx)
			return
		}
		for j := 0; j < k; j++ {
			if cnt[j]+jobs[i] >= ans {
				continue
			}
			cnt[j] += jobs[i]
			dfs(i + 1)
			cnt[j] -= jobs[i]
			if cnt[j] == 0 {
				break
			}
		}
	}
	dfs(0)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
