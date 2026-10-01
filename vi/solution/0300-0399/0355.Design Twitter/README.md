---
comments: true
difficulty: Medium
tags:
    - Design
    - Hash Table
    - Linked List
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [355. Design Twitter](https://leetcode.com/problems/design-twitter)

[中文文档](/solution/0300-0399/0355.Design%20Twitter/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế phiên bản đơn giản của Twitter, nơi người dùng có thể đăng tweet, follow/unfollow người khác và xem <code>10</code> tweet mới nhất trong news feed của mình.</p>

<p>Hãy triển khai class <code>Twitter</code>:</p>

<ul>
	<li><code>Twitter()</code> khởi tạo đối tượng Twitter.</li>
	<li><code>void postTweet(int userId, int tweetId)</code> tạo tweet mới có ID <code>tweetId</code> do người dùng <code>userId</code> đăng. Mỗi lần gọi hàm này sẽ có <code>tweetId</code> duy nhất.</li>
	<li><code>List&lt;Integer&gt; getNewsFeed(int userId)</code> lấy ID của <code>10</code> tweet mới nhất trong news feed của người dùng. Mỗi tweet phải do người dùng follow hoặc chính người dùng đó đăng. Tweet phải được <strong>sắp xếp từ mới nhất đến cũ nhất</strong>.</li>
	<li><code>void follow(int followerId, int followeeId)</code> người dùng có ID <code>followerId</code> bắt đầu follow người dùng có ID <code>followeeId</code>.</li>
	<li><code>void unfollow(int followerId, int followeeId)</code> người dùng có ID <code>followerId</code> ngừng follow người dùng có ID <code>followeeId</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Twitter&quot;, &quot;postTweet&quot;, &quot;getNewsFeed&quot;, &quot;follow&quot;, &quot;postTweet&quot;, &quot;getNewsFeed&quot;, &quot;unfollow&quot;, &quot;getNewsFeed&quot;]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
<strong>Đầu ra</strong>
[null, null, [5], null, null, [6, 5], null, [5]]

<strong>Giải thích</strong>
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
twitter.getNewsFeed(1);  // User 1&#39;s news feed should return a list with 1 tweet id -&gt; [5]. return [5]
twitter.follow(1, 2);    // User 1 follows user 2.
twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
twitter.getNewsFeed(1);  // User 1&#39;s news feed should return a list with 2 tweet ids -&gt; [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.unfollow(1, 2);  // User 1 unfollows user 2.
twitter.getNewsFeed(1);  // User 1&#39;s news feed should return a list with 1 tweet id -&gt; [5], since user 1 is no longer following user 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= userId, followerId, followeeId &lt;= 500</code></li>
	<li><code>0 &lt;= tweetId &lt;= 10<sup>4</sup></code></li>
	<li>ID của tất cả tweet đều <strong>duy nhất</strong>.</li>
	<li>Sẽ có tối đa <code>3 * 10<sup>4</sup></code> lần gọi các hàm <code>postTweet</code>, <code>getNewsFeed</code>, <code>follow</code> và <code>unfollow</code>.</li>
	<li>Người dùng không thể tự follow chính mình.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần hỗ trợ đăng tweet, follow và lấy mười tweet mới nhất của người dùng cùng những người họ follow. Quét toàn bộ tweet sẽ tốn kém.
>
> Lưu tweet và danh sách follow của từng người dùng, đồng thời gắn timestamp cho mỗi tweet bằng một global clock. Khi tạo news feed, lấy tối đa mười tweet mới nhất từ mỗi người liên quan rồi giữ lại mười tweet mới nhất toàn bộ. Thao tác follow/unfollow chỉ cần cập nhật set.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Twitter:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.user_tweets = defaultdict(list)
        self.user_following = defaultdict(set)
        self.tweets = defaultdict()
        self.time = 0

    def postTweet(self, userId: int, tweetId: int) -> None:
        """
        Compose a new tweet.
        """
        self.time += 1
        self.user_tweets[userId].append(tweetId)
        self.tweets[tweetId] = self.time

    def getNewsFeed(self, userId: int) -> List[int]:
        """
        Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
        """
        following = self.user_following[userId]
        users = set(following)
        users.add(userId)
        tweets = [self.user_tweets[u][::-1][:10] for u in users]
        tweets = sum(tweets, [])
        return nlargest(10, tweets, key=lambda tweet: self.tweets[tweet])

    def follow(self, followerId: int, followeeId: int) -> None:
        """
        Follower follows a followee. If the operation is invalid, it should be a no-op.
        """
        self.user_following[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        """
        Follower unfollows a followee. If the operation is invalid, it should be a no-op.
        """
        following = self.user_following[followerId]
        if followeeId in following:
            following.remove(followeeId)


# Your Twitter object will be instantiated and called as such:
# obj = Twitter()
# obj.postTweet(userId,tweetId)
# param_2 = obj.getNewsFeed(userId)
# obj.follow(followerId,followeeId)
# obj.unfollow(followerId,followeeId)
```

#### Java

```java
class Twitter {
    private Map<Integer, List<Integer>> userTweets;
    private Map<Integer, Set<Integer>> userFollowing;
    private Map<Integer, Integer> tweets;
    private int time;

    /** Initialize your data structure here. */
    public Twitter() {
        userTweets = new HashMap<>();
        userFollowing = new HashMap<>();
        tweets = new HashMap<>();
        time = 0;
    }

    /** Compose a new tweet. */
    public void postTweet(int userId, int tweetId) {
        userTweets.computeIfAbsent(userId, k -> new ArrayList<>()).add(tweetId);
        tweets.put(tweetId, ++time);
    }

    /**
     * Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed
     * must be posted by users who the user followed or by the user herself. Tweets must be ordered
     * from most recent to least recent.
     */
    public List<Integer> getNewsFeed(int userId) {
        Set<Integer> following = userFollowing.getOrDefault(userId, new HashSet<>());
        Set<Integer> users = new HashSet<>(following);
        users.add(userId);
        PriorityQueue<Integer> pq
            = new PriorityQueue<>(10, (a, b) -> (tweets.get(b) - tweets.get(a)));
        for (Integer u : users) {
            List<Integer> userTweet = userTweets.get(u);
            if (userTweet != null && !userTweet.isEmpty()) {
                for (int i = userTweet.size() - 1, k = 10; i >= 0 && k > 0; --i, --k) {
                    pq.offer(userTweet.get(i));
                }
            }
        }
        List<Integer> res = new ArrayList<>();
        while (!pq.isEmpty() && res.size() < 10) {
            res.add(pq.poll());
        }
        return res;
    }

    /** Follower follows a followee. If the operation is invalid, it should be a no-op. */
    public void follow(int followerId, int followeeId) {
        userFollowing.computeIfAbsent(followerId, k -> new HashSet<>()).add(followeeId);
    }

    /** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
    public void unfollow(int followerId, int followeeId) {
        userFollowing.computeIfAbsent(followerId, k -> new HashSet<>()).remove(followeeId);
    }
}

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter obj = new Twitter();
 * obj.postTweet(userId,tweetId);
 * List<Integer> param_2 = obj.getNewsFeed(userId);
 * obj.follow(followerId,followeeId);
 * obj.unfollow(followerId,followeeId);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
