# Popular Posts

Monday, July 01, 2013
7:27 AM

**How to Build a Popularity Algorithm You can be Proud of**\
Pasted from <[http://blog.linkibol.com/2010/05/07/how-to-build-a-popularity-algorithm-you-can-be-proud-of/](http://blog.linkibol.com/2010/05/07/how-to-build-a-popularity-algorithm-you-can-be-proud-of/)>

**How Hacker News ranking algorithm works**\
Pasted from <[http://amix.dk/blog/post/19574](http://amix.dk/blog/post/19574)>

**How Reddit ranking algorithms work**\
Pasted from <[http://amix.dk/blog/post/19588](http://amix.dk/blog/post/19588)>

SELECT\
Title\
, DateAdded\
, WebCount\
FROM\
subtext_EntryViewCount evc\
INNER JOIN subtext_Content c\
ON evc.EntryID = c.ID\
ORDER BY\
WebCount DESC
