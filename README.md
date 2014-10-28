# Security Fix for Ivan's Blog

XSS:
```
http://localhost:3000/posts?utf8=%E2%9C%93&search=archive&status=foo=%22bar%22%3E%3Cscript%3Ealert%28%22p0wned!!!%22%29%3C/script%3E%3Cp%20data-foo
```

Fixed by turning on XSS Protection: `response.headers['X-XSS-Protection'] = '1'`

SQL Injection:

```
foo%'); INSERT INTO posts (id,title,body,created_at,updated_at) VALUES (99,'hacked','hacked alright','2013-07-18','2013-07-18'); SELECT "posts".* FROM "posts" WHERE (title like 'hacked%
```

Fixed by escaping input in the posts model: `includes(comments: :replies).where("title like ?", "%#{search}%")`.


Strong Params:

Added strong params in both the posts and comments controller.
