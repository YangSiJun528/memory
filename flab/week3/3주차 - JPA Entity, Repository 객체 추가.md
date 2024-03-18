- - -
### JPA Entity, Repository 추가

구상한 대로 JPA Entity를 구현하고, JPA Repository를 생성하였다.

- [Isuue](https://github.com/f-lab-edu/celog/issues/4)
- [PR](https://github.com/f-lab-edu/celog/pull/5)

### 추가 자료

![](flab/week3/erd-v2.png)

DBML 언어로 표시
```DBML
Table post {
  post_id int [primary key]
  user_id int [ref: > user.user_id, not null]
  title varchar [not null]
  content varchar [not null]
  publication_status varchar [not null, note: "public_published | follower_published | drafting"]
  create_at varchar [not null]
  update_at varchar [not null]
}

Table user {
  user_id int [primary key]
  username varchar [not null]
  email varchar [null, unique]
  profile_url varchar [null]
  oauth_provider_id varchar [null, note: "email, oauth_provider_id 둘 중 하나는 무조건 not null이여야 한다. 이유: github OAuth는 email이 없을 수 있음"]
  auth_type varchar [not null, note: "github | google | email"]
  create_at varchar [not null]
  update_at varchar [not null]
}

Table reply {
  reply_id int [primary key]
  user_id int [ref: > user.user_id, not null]
  post_id int [ref: > post.post_id, not null]
  content varchar [not null]
  super_reply_id int [ref: > reply.reply_id, null] 
  create_at varchar [not null]
  update_at varchar [not null]
}

Table follow {
  follow_id int [primary key]
  follower_id int [ref: > user.user_id, not null, note: "팔로우 하는 사용자의 ID"] 
  following_id int [ref: > user.user_id, not null, note: "팔로우 받는 사용자의 ID"]
  create_at varchar [not null]
}

Table bookmark {
  bookmark_id int [primary key]
  user_id int [ref: > user.user_id, not null]
  post_id int [ref: > post.post_id, not null]
  create_at varchar [not null]
}

Table hashtag {
  hashtag_id int [primary key]
  tag_name varchar [not null, unique]
}

Table post_hashtag {
  post_hashtag_id int [primary key]
  post_id int [ref: > post.post_id, not null]
  hashtag_id int [ref: > hashtag.hashtag_id, not null]
}

Table notification {
  notification_id int [primary key]
  user_id int [ref: > user.user_id, not null]
  content varchar [not null]
  read_status boolean [not null, default: false]
  create_at varchar [not null]
}
```
