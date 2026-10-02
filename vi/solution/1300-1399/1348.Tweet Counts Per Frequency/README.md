---
comments: true
difficulty: Medium
rating: 2036
source: Weekly Contest 175 Q3
tags:
    - Design
    - Hash Table
    - String
    - Binary Search
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [1348. Tweet Counts Per Frequency](https://leetcode.com/problems/tweet-counts-per-frequency)

[中文文档](/solution/1300-1399/1348.Tweet%20Counts%20Per%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Một công ty mạng xã hội muốn theo dõi hoạt động trên trang bằng cách phân tích số tweet trong các khoảng thời gian được chọn. Có thể chia mỗi khoảng thời gian thành các <strong>time chunk</strong> nhỏ hơn theo tần suất nhất định (mỗi <strong>phút</strong>, <strong>giờ</strong> hoặc <strong>ngày</strong>).</p>

<p>Ví dụ, khoảng thời gian <code>[10, 10000]</code> (tính bằng <strong>giây</strong>) được chia thành các <strong>time chunk</strong> như sau theo từng tần suất:</p>

<ul>
	<li>Mỗi <strong>phút</strong> (các chunk 60 giây): <code>[10,69]</code>, <code>[70,129]</code>, <code>[130,189]</code>, <code>...</code>, <code>[9970,10000]</code></li>
	<li>Mỗi <strong>giờ</strong> (các chunk 3600 giây): <code>[10,3609]</code>, <code>[3610,7209]</code>, <code>[7210,10000]</code></li>
	<li>Mỗi <strong>ngày</strong> (các chunk 86400 giây): <code>[10,10000]</code></li>
</ul>

<p>Lưu ý chunk cuối cùng có thể ngắn hơn độ dài chunk theo tần suất đã chọn và luôn kết thúc tại thời điểm cuối của khoảng thời gian (<code>10000</code> trong ví dụ trên).</p>

<p>Hãy thiết kế và triển khai một API hỗ trợ công ty phân tích dữ liệu.</p>

<p>Triển khai class <code>TweetCounts</code>:</p>

<ul>
	<li><code>TweetCounts()</code> Khởi tạo đối tượng <code>TweetCounts</code>.</li>
	<li><code>void recordTweet(String tweetName, int time)</code> Lưu <code>tweetName</code> tại thời điểm <code>time</code> được ghi nhận (tính bằng <strong>giây</strong>).</li>
	<li><code>List&lt;Integer&gt; getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime)</code> Trả về danh sách số nguyên biểu thị số tweet có tên <code>tweetName</code> trong mỗi <strong>time chunk</strong> thuộc khoảng thời gian <code>[startTime, endTime]</code> (tính bằng <strong>giây</strong>) với tần suất <code>freq</code>.
	<ul>
		<li><code>freq</code> nhận một trong các giá trị <code>&quot;minute&quot;</code>, <code>&quot;hour&quot;</code> hoặc <code>&quot;day&quot;</code>, lần lượt tương ứng với tần suất mỗi <strong>phút</strong>, <strong>giờ</strong> hoặc <strong>ngày</strong>.</li>
	</ul>
	</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;TweetCounts&quot;,&quot;recordTweet&quot;,&quot;recordTweet&quot;,&quot;recordTweet&quot;,&quot;getTweetCountsPerFrequency&quot;,&quot;getTweetCountsPerFrequency&quot;,&quot;recordTweet&quot;,&quot;getTweetCountsPerFrequency&quot;]
[[],[&quot;tweet3&quot;,0],[&quot;tweet3&quot;,60],[&quot;tweet3&quot;,10],[&quot;minute&quot;,&quot;tweet3&quot;,0,59],[&quot;minute&quot;,&quot;tweet3&quot;,0,60],[&quot;tweet3&quot;,120],[&quot;hour&quot;,&quot;tweet3&quot;,0,210]]

<strong>Đầu ra</strong>
[null,null,null,null,[2],[2,1],null,[4]]

<strong>Giải thích</strong>
TweetCounts tweetCounts = new TweetCounts();
tweetCounts.recordTweet(&quot;tweet3&quot;, 0);                              // New tweet &quot;tweet3&quot; at time 0
tweetCounts.recordTweet(&quot;tweet3&quot;, 60);                             // New tweet &quot;tweet3&quot; at time 60
tweetCounts.recordTweet(&quot;tweet3&quot;, 10);                             // New tweet &quot;tweet3&quot; at time 10
tweetCounts.getTweetCountsPerFrequency(&quot;minute&quot;, &quot;tweet3&quot;, 0, 59); // return [2]; chunk [0,59] had 2 tweets
tweetCounts.getTweetCountsPerFrequency(&quot;minute&quot;, &quot;tweet3&quot;, 0, 60); // return [2,1]; chunk [0,59] had 2 tweets, chunk [60,60] had 1 tweet
tweetCounts.recordTweet(&quot;tweet3&quot;, 120);                            // New tweet &quot;tweet3&quot; at time 120
tweetCounts.getTweetCountsPerFrequency(&quot;hour&quot;, &quot;tweet3&quot;, 0, 210);  // return [4]; chunk [0,210] had 4 tweets
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= time, startTime, endTime &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= endTime - startTime &lt;= 10<sup>4</sup></code></li>
	<li>Tổng cộng có tối đa <code>10<sup>4</sup></code> lần gọi <code>recordTweet</code> và <code>getTweetCountsPerFrequency</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lưu thời điểm tweet theo từng user, rồi đếm tweet trong các bucket phút/giờ/ngày thuộc khoảng $[\textit{start},\textit{end}]$. Mỗi thao tác có thể được gọi tới $10^4$ lần, nên quét toàn bộ tweet cho từng query sẽ tốn kém. Với danh sách đã sắp xếp cho mỗi user, thao tác chèn mất $O(\log n)$; mỗi bucket được đếm bằng hai lần tìm kiếm nhị phân trong khoảng $[t,\min(t+f,\textit{end}+1))$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class TweetCounts:
    def __init__(self):
        self.d = {"minute": 60, "hour": 3600, "day": 86400}
        self.data = defaultdict(SortedList)

    def recordTweet(self, tweetName: str, time: int) -> None:
        self.data[tweetName].add(time)

    def getTweetCountsPerFrequency(
        self, freq: str, tweetName: str, startTime: int, endTime: int
    ) -> List[int]:
        f = self.d[freq]
        tweets = self.data[tweetName]
        t = startTime
        ans = []
        while t <= endTime:
            l = tweets.bisect_left(t)
            r = tweets.bisect_left(min(t + f, endTime + 1))
            ans.append(r - l)
            t += f
        return ans


# Your TweetCounts object will be instantiated and called as such:
# obj = TweetCounts()
# obj.recordTweet(tweetName,time)
# param_2 = obj.getTweetCountsPerFrequency(freq,tweetName,startTime,endTime)
```

#### Java

```java
class TweetCounts {
    private Map<String, TreeMap<Integer, Integer>> data = new HashMap<>();

    public TweetCounts() {
    }

    public void recordTweet(String tweetName, int time) {
        data.putIfAbsent(tweetName, new TreeMap<>());
        var tm = data.get(tweetName);
        tm.put(time, tm.getOrDefault(time, 0) + 1);
    }

    public List<Integer> getTweetCountsPerFrequency(
        String freq, String tweetName, int startTime, int endTime) {
        int f = 60;
        if ("hour".equals(freq)) {
            f = 3600;
        } else if ("day".equals(freq)) {
            f = 86400;
        }
        var tm = data.get(tweetName);
        List<Integer> ans = new ArrayList<>();
        for (int i = startTime; i <= endTime; i += f) {
            int s = 0;
            int end = Math.min(i + f, endTime + 1);
            for (int v : tm.subMap(i, end).values()) {
                s += v;
            }
            ans.add(s);
        }
        return ans;
    }
}

/**
 * Your TweetCounts object will be instantiated and called as such:
 * TweetCounts obj = new TweetCounts();
 * obj.recordTweet(tweetName,time);
 * List<Integer> param_2 = obj.getTweetCountsPerFrequency(freq,tweetName,startTime,endTime);
 */
```

#### C++

```cpp
class TweetCounts {
public:
    TweetCounts() {
    }

    void recordTweet(string tweetName, int time) {
        data[tweetName].insert(time);
    }

    vector<int> getTweetCountsPerFrequency(string freq, string tweetName, int startTime, int endTime) {
        int f = 60;
        if (freq == "hour")
            f = 3600;
        else if (freq == "day")
            f = 86400;
        vector<int> ans((endTime - startTime) / f + 1);
        auto l = data[tweetName].lower_bound(startTime);
        auto r = data[tweetName].upper_bound(endTime);
        for (; l != r; ++l) {
            ++ans[(*l - startTime) / f];
        }
        return ans;
    }

private:
    unordered_map<string, multiset<int>> data;
};

/**
 * Your TweetCounts object will be instantiated and called as such:
 * TweetCounts* obj = new TweetCounts();
 * obj->recordTweet(tweetName,time);
 * vector<int> param_2 = obj->getTweetCountsPerFrequency(freq,tweetName,startTime,endTime);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
