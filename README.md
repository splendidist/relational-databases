# relational-databases

## Create tables

```sql
CREATE TABLE IF NOT EXISTS categories (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL
)

CREATE TABLE IF NOT EXISTS posts (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  content TEXT NOT NULL,
  category_id INTEGER REFERENCES categories(id)
)

CREATE TABLE IF NOT EXISTS tags (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL
)

CREATE TABLE IF NOT EXISTS posts_tags (
  post_id INTEGER REFERENCES posts(id),
  tag_id INTEGER REFERENCES tags(id),
  PRIMARY KEY (post_id, tag_id)
)
```

## Add data

```sql
INSERT INTO categories (name) VALUES
('Technology'),
('Lifestyle'),
('Education');

INSERT INTO posts (title, content, category_id) VALUES
('Introduction to Node.js', 'Node.js is a JavaScript runtime...', 1),
('Daily Yoga Practices', 'Yoga is beneficial for health...', 2),
('Learning SQL Basics', 'SQL is a standard language for...', 3);

INSERT INTO tags (name) VALUES
('Programming'),
('Health'),
('Education');
```

## Add tags/junction table

```sql
INSERT INTO posts_tags (post_id, tag_id) VALUES
(1, 1),
(2, 2),
(3, 3);
```

## Select data

```sql
SELECT
posts.title,
posts.content,
categories.name AS category
FROM posts
JOIN categories ON posts.category_id = categories.id;

-- by a specific id

SELECT posts.title,
posts.content,
categories.name AS category
FROM posts
JOIN categories ON posts.category_id = categories.id
WHERE categories.id = 1;

-- with tags

SELECT
posts.title,
posts.content,
categories.name AS category,
tags.name AS tag
FROM posts
JOIN categories ON posts.category_id = categories.id
JOIN posts_tags ON posts.id = posts_tags.post_id
JOIN tags ON posts_tags.tag_id = tags.id;

-- title and tags

SELECT posts.title, ARRAY_AGG(tags.name) AS tags
FROM posts
JOIN posts_tags ON posts.id = posts_tags.post_id
JOIN tags ON posts_tags.tag_id = tags.id
GROUP BY posts.title;
```

## Add a new tag

```sql
INSERT INTO posts_tags(post_id, tag_id) VALUES(1, 2);
```
