# Snippet

To get recent passengers that `passenger_id%10=5` and have any rides in recent 30 days for services 1, 2 or 3.

```sql
WITH 
	groupArray(created_at) as time_sorted_vals,
	groupArray(id) as ride_ids_vals,
	groupArray(origin_lat) as origin_lat_vals,
	groupArray(origin_lng) as origin_lon_vals,
	groupArray(destination_lat) as dest_lat_vals,
	groupArray(destination_lng) as dest_lon_vals,
	arrayEnumerate(time_sorted_vals) as indexes,
	arrayMap( i -> time_sorted_vals[i] - time_sorted_vals[i-1], indexes) as running_diffs,
	arrayMap( i -> ride_ids_vals[i] , indexes) as ride_ids,
	arrayMap( i -> origin_lat_vals[i] , indexes) as origin_lats,
	arrayMap( i -> origin_lon_vals[i] , indexes) as origin_lons,
	arrayMap( i -> dest_lat_vals[i] , indexes) as dest_lats,
	arrayMap( i -> dest_lon_vals[i] , indexes) as dest_lons
SELECT 
	passenger_id,
	ride_ids,
	origin_lats,
	origin_lons,
	dest_lats,
	dest_lons,
	running_diffs
FROM 
	(
		SELECT 
			id, passenger_id, origin_lat, origin_lng, destination_lat, destination_lng, created_at, created_date
		FROM rides_view as r
		WHERE 
			r.latest_ride_status = 5
			AND r.passenger_id %% 10 = 4
			AND (service_type = 1 OR service_type = 2 OR service_type = 3)
			AND r.created_date > toDate(%(last_ride_created_date)s) - %(looking_range)s
		ORDER BY created_at DESC
	)
GROUP by passenger_id
```

## Huddle Database

This assignment is for database laboratory course. The project is in `nebula-clans/huddle-server` in github.

```sql
-- Select all of the posts
select *
from posts_post;

-- Select the post with the most likes
select p.id, p.title, count(*) as likes
from posts_post as p
         inner join likes_postlike lp on p.id = lp.post_id
group by p.id
order by likes desc
limit 1;

-- Select all of your followers posts
select p.id, date_created, author_id
from posts_post as p
where author_id in (
    select fu.following_user_id
    from authentication_user as u
             inner join follow_userfollowing fu on u.id = fu.user_id
    where u.id = 3
);

-- Select all of the replies to a comment
select *
from comment_comment as c
         inner join comment_commentreply cc on c.id = cc.reply_id
where cc.reply_to_id = 1;

-- Select all of the posts from a user in a single community
select *
from posts_post as p
         inner join community_community cc on cc.id = p.community_id
where cc.id = 1
  and p.author_id = 1;

-- Select trending hashtags in recent 30 days
select hh.id, hh.text, count(p.id) as count_hashtag
from posts_post as p
         inner join hashtag_posthashtag hp on p.id = hp.post_id
         inner join hashtag_hashtag hh on hh.id = hp.hashtag_id
where p.date_created > now() - interval '30 days'
group by hh.id
order by count_hashtag desc
limit 10;

-- Select post with `golang` hashtag
select *
from posts_post as p
         inner join hashtag_posthashtag hp on p.id = hp.post_id
         inner join hashtag_hashtag hh on hh.id = hp.hashtag_id
where hh.text = 'golang'
   or hh.text = 'first_post';

-- Select 3 comment of the post with the most likes from other users
select c.id, c.author_id, c.content, count(*) as likes
from comment_comment as c
         inner join likes_commentlike lc on c.id = lc.comment_id
group by c.id
order by likes desc
limit 3;

-- Select posts in tech category that has go in their title or description.
select *
from posts_post as p
where p.category_id = 'tech'
  and (p.title like '%go%' or p.description like '%go%');

-- Select users where don't have logged in the account in 30 days and don't publish any posts.
select *
from authentication_user as u
where u.date_joined < now() - interval '180 days';

-- Select the most reported post with content in last 2 weeks
select pc.id, pc.content_text, p.reports_number
from posts_post as p
         inner join posts_content pc on pc.id = p.post_content_id
where p.date_created > now() - interval '14 days'
order by p.reports_number desc;

-- Select all of the followers current comments (2 weeks)
select *
from comment_comment as c
where c.author_id in (
    select fu.following_user_id
    from authentication_user as u
             inner join follow_userfollowing fu on u.id = fu.user_id
    where u.id = 3
)
  and c.create_date > now() - interval '14 days';

-- Select each user contribution in the site categories ordered by the most contribution
select au.username, cc.name, count(p.id) as count
from posts_post as p
         inner join authentication_user au on au.id = p.author_id
         inner join category_category cc on cc.name = p.category_id
group by au.username, cc.name
order by count desc;

-- Make a report of all posts in all categories and show the number of
-- likes and comments in that category which are ordered by the count of likes
-- first and then with the count of comments.
select cc.name,
       (
           select count(lp.id) as likes
           from posts_post as p
                    inner join category_category ccc on ccc.name = p.category_id
                    inner join likes_postlike lp on p.id = lp.post_id
           where p.category_id = cc.name
           group by ccc.name
       ) as likes,
       (
           select count(cmnt.id) as comment_count
           from posts_post as p
                    inner join category_category ccc on ccc.name = p.category_id
                    inner join comment_comment cmnt on p.author_id = cmnt.author_id
           where p.category_id = cc.name
           group by ccc.name
       ) as comment_count
from posts_post as p
         inner join category_category cc on cc.name = p.category_id
         inner join comment_comment c on p.author_id = c.author_id
         inner join likes_postlike lp on p.id = lp.post_id
group by cc.name
order by likes desc, comment_count desc;
```