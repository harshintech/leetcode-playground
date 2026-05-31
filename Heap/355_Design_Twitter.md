# 🐦 Design Twitter

---

## 📌 Problem

Design a simplified Twitter system that supports:

```java
postTweet(userId, tweetId)
getNewsFeed(userId)
follow(followerId, followeeId)
unfollow(followerId, followeeId)
```

Requirements:

- Users can post tweets.
- Users can follow/unfollow others.
- News Feed should return the 10 most recent tweets.
- Tweets from:
  - User himself
  - All followed users

---

```java
class Twitter {

    private static int timeStamp = 0;

    Map<Integer, List<Tweet>> tweetMap;

    private class Tweet {
        public int tweetId;
        public int time;
        public Tweet next;

        public Tweet(int tweetId) {
            this.tweetId = tweetId;
            time = timeStamp++;
            next = null;
        }
    }

    class User {
        int userId;
        Set<Integer> followed;
        //you can write this also Set<Integer> followed = new HashSet<>();
        Tweet tweetHead;

        User(int userId) {
            this.userId = userId;
            followed = new HashSet<>(); //Author chahta hai ki object create hote hi saari initialization constructor me dikhe.
            follow(userId); //follow self
        }

        void follow(int id) {
            followed.add(id);
        }

        void unfollow(int id) {
            if (id != userId) {
                followed.remove(id);
            }
        }

        void post(int tweetId) {
            Tweet tweet = new Tweet(tweetId);
            tweet.next = tweetHead;
            tweetHead = tweet;
        }

        /** Suppose user 1 post 5,6,8 post
            tweetHead
             |
             v
            [8] -> [6] -> [5] -> null
         */
    }

    Map<Integer, User> userMap;

    public Twitter() {
        userMap = new HashMap<>();
    }

    public void postTweet(int userId, int tweetId) {
        if (!userMap.containsKey(userId)) {
            userMap.put(userId, new User(userId));
        }
        userMap.get(userId).post(tweetId);
    }

    public List<Integer> getNewsFeed(int userId) {
        List<Integer> res = new ArrayList<>();

        if(!userMap.containsKey(userId)){
            return res;
        }

        Set<Integer> users = userMap.get(userId).followed;
        PriorityQueue<Tweet> pq = new PriorityQueue<>((a,b) -> b.time - a.time);

        for(int id: users){
            Tweet tweet = userMap.get(id).tweetHead;

            if(tweet != null){
                pq.offer(tweet);
            }
        }

        while(!pq.isEmpty() && res.size() < 10){
            Tweet curr = pq.poll();

            res.add(curr.tweetId);

            if(curr.next != null){
                pq.offer(curr.next);
            }
        }

        return res;
    }

    public void follow(int followerId, int followeeId) {
        if (!userMap.containsKey(followerId)) {
            userMap.put(followerId, new User(followerId));
        }

        if (!userMap.containsKey(followeeId)) {
            userMap.put(followeeId, new User(followeeId));
        }

        userMap.get(followerId).follow(followeeId);
    }

    public void unfollow(int followerId, int followeeId) {
         if (!userMap.containsKey(followerId)) {
            return;
        }

        userMap.get(followerId).unfollow(followeeId);
    }
}
```

# 🏗 Data Structures Used

```java
Map<Integer, User> userMap
```

Stores:

```text
userId → User Object
```

---

## User Class

```java
class User
```

Contains:

```java
int userId
Set<Integer> followed
Tweet tweetHead
```

---

### followed

Stores:

```text
All users this user follows
```

Example:

```text
User 1 follows:
1,2,3
```

```java
followed = {1,2,3}
```

---

### tweetHead

Head of tweet linked list.

Newest tweet always at front.

Example:

User posts:

```text
5
6
8
```

Linked List:

```text
tweetHead
   |
   v
[8] → [6] → [5] → null
```

---

# 🐦 Tweet Class

```java
class Tweet
```

Contains:

```java
tweetId
time
next
```

---

Example:

```text
Tweet 5
```

```text
tweetId = 5
time = 0
next = null
```

---

# ⏰ Global Timestamp

```java
private static int timeStamp = 0;
```

Every new tweet gets:

```java
time = timeStamp++;
```

Example:

```text
Tweet 5 → time=0
Tweet 6 → time=1
Tweet 8 → time=2
```

---

# 📝 postTweet()

```java
postTweet(userId, tweetId)
```

---

## If user doesn't exist

```java
userMap.put(userId,new User(userId))
```

Create user.

---

## Create Tweet

```java
Tweet tweet = new Tweet(tweetId);
```

---

## Insert At Head

```java
tweet.next = tweetHead;
tweetHead = tweet;
```

---

Example

Post:

```text
5
6
8
```

Result:

```text
[8] → [6] → [5]
```

Newest tweet always first.

---

# 👥 follow()

```java
follow(followerId, followeeId)
```

---

Example

```java
follow(1,2);
```

Meaning:

```text
User 1 follows User 2
```

Stored:

```java
user1.followed.add(2)
```

---

## Self Follow

Inside constructor:

```java
follow(userId);
```

Every user automatically follows himself.

---

Example

User 1:

```text
followed = {1}
```

Why?

So his own tweets appear in feed.

---

# 🚫 unfollow()

```java
unfollow(followerId, followeeId)
```

---

Important:

```java
if(id != userId)
```

A user cannot unfollow himself.

---

Example

Invalid:

```java
unfollow(1,1)
```

Ignored.

---

# 📰 getNewsFeed()

Most important function.

---

Goal:

```text
Return latest 10 tweets
```

from:

```text
Self
+
All followed users
```

---

# Step 1

Get followed users.

```java
Set<Integer> users =
userMap.get(userId).followed;
```

Example:

```text
{1,2,3}
```

---

# Step 2

Create Max Heap

```java
PriorityQueue<Tweet> pq =
new PriorityQueue<>(
(a,b)-> b.time-a.time
);
```

Largest timestamp first.

---

Example

```text
time=10
time=8
time=5
```

Top:

```text
time=10
```

---

# Step 3

Insert Head Tweet Of Every User

```java
for(int id : users)
```

Example:

```text
User1:
8 → 6 → 5

User2:
11 → 9

User3:
20 → 15
```

Heap initially:

```text
8
11
20
```

Actually stored by timestamp.

---

# Why Only Head Tweet?

Because:

```text
Head = newest tweet
```

Older tweets are reachable using:

```java
curr.next
```

---

# 🔥 Main Trick

This is exactly:

```text
Merge K Sorted Linked Lists
```

---

Each user's tweet list is already sorted:

```text
Newest
↓
Older
↓
Older
```

Example:

```text
20 → 15
11 → 9
8 → 6 → 5
```

---

# Step 4

Pop Latest Tweet

```java
Tweet curr = pq.poll();
```

Add to answer.

```java
res.add(curr.tweetId);
```

---

# Step 5

Push Next Tweet

```java
if(curr.next != null)
    pq.offer(curr.next);
```

---

Example

Pop:

```text
20
```

Push:

```text
15
```

Heap still contains latest available tweets.

---

# Dry Run

---

User1 tweets:

```text
8 → 6 → 5
```

User2 tweets:

```text
11 → 9
```

---

Initial Heap

```text
8
11
```

Top:

```text
11
```

---

Pop:

```text
11
```

Feed:

```text
[11]
```

Push:

```text
9
```

Heap:

```text
9
8
```

---

Pop:

```text
9
```

Feed:

```text
[11,9]
```

---

Pop:

```text
8
```

Feed:

```text
[11,9,8]
```

Push:

```text
6
```

---

Continue...

Result:

```text
[11,9,8,6,5]
```

---

# 🧠 Why Priority Queue?

Without heap:

```text
Collect all tweets
Sort
Take top 10
```

Very expensive.

---

Heap lets us always get:

```text
Most recent tweet
```

in:

```text
O(log k)
```

where:

```text
k = followed users
```

---

# ⏱ Complexity

## postTweet()

```text
O(1)
```

---

## follow()

```text
O(1)
```

---

## unfollow()

```text
O(1)
```

---

## getNewsFeed()

Let:

```text
F = followed users
```

Heap size:

```text
F
```

At most:

```text
10 tweets removed
```

Complexity:

```text
O(10 log F)
```

≈

```text
O(log F)
```

---

# 🏆 Pattern Recognition

This problem combines:

```text
HashMap
Linked List
Heap
Design
```

Most important pattern:

```text
Merge K Sorted Lists using Priority Queue
```

---

# 🔥 Golden Mental Model

```text
Each user keeps tweets as a linked list.

News Feed starts with newest tweet
from every followed user.

Heap always gives the globally newest tweet.

After removing one tweet,
insert its next older tweet.

Repeat until 10 tweets are collected.
```

---

# 🚀 Final Summary

```text
User
 ├── Followed Users (HashSet)
 └── Tweet Linked List

Twitter
 └── userId → User (HashMap)

News Feed
 └── Max Heap
       +
       Merge K Sorted Tweet Lists
```

This is why the solution is accepted with optimal performance.

