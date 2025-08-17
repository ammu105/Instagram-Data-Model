# 📸 Instagram Data Modaling (MySQL)
## About the Project
This project is a simple Instagram-style database built with MySQL.
It covers the main features of Instagram like **users, posts, likes, comments, and followers**.

I designed the data model, created tables with relationships, inserted sample data, and wrote queries to do analytics like counting likes, ranking posts, and finding active users.
# 🗂 Database Schema
## Entities:
- **Users** → Stores user details.
- **Posts** → Stores user posts with captions and images.
- **Comments** → Stores comments made by users on posts.
- **Likes** → Stores likes for posts.
- **Followers** → Stores follower relationships between users.
# 📌 ER Diagram
![ER diagram of Instagram](https://github.com/ammu105/Instagram-Data-Model/blob/main/Instagram_ER_diagram) 
# 🛠️ Database Setup
## 1.Create Database

```md
sql

CREATE DATABASE instagram;
USE instagram;
```
## 2. Create Tables
```
sql

CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(20) UNIQUE
);

CREATE TABLE Posts (
    post_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    caption TEXT,
    image_url VARCHAR(200),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Comments (
    comment_id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    comment_text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Likes (
    like_id INT AUTO_INCREMENT PRIMARY KEY,
    post_id INT NOT NULL,
    user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (post_id) REFERENCES Posts(post_id),
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
);

CREATE TABLE Followers (
    follower_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    follower_user_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (follower_user_id) REFERENCES Users(user_id)
);
```
## 📥 Sample Data
```
sql

-- Insert Users
INSERT INTO Users (name, email, phone_number)
VALUES
('John Smith', 'johnsmith@gmail.com', '1234567890'),
('Jane Doe', 'janedoe@yahoo.com', '0987654321'),
('Bob Johnson', 'bjohnson@gmail.com', '1112223333'),
('Alice Brown', 'abrown@yahoo.com', NULL),
('Mike Davis', 'mdavis@gmail.com', '5556667777');

-- Insert Posts
INSERT INTO Posts (user_id, caption, image_url)
VALUES
(1, 'Beautiful sunset', 'https://www.example.com/sunset.jpg'),
(2, 'My new puppy', 'https://www.example.com/puppy.jpg'),
(3, 'Delicious pizza', 'https://www.example.com/pizza.jpg'),
(4, 'Throwback to my vacation', 'https://www.example.com/vacation.jpg'),
(5, 'Amazing concert', 'https://www.example.com/concert.jpg');

-- Insert Comments
INSERT INTO Comments (post_id, user_id, comment_text)
VALUES
(1, 2, 'Wow! Stunning.'),
(1, 3, 'Beautiful colors.'),
(2, 1, 'What a cutie!'),
(2, 4, 'Aww, I want one.'),
(3, 5, 'Yum!'),
(4, 1, 'Looks like an awesome trip.'),
(5, 3, 'Wish I was there!');

-- Insert Likes
INSERT INTO Likes (post_id, user_id)
VALUES
(1, 2), (1, 4), (2, 1), (2, 3), (3, 5),
(4, 1), (4, 2), (4, 3), (5, 4), (5, 5);

-- Insert Followers
INSERT INTO Followers (user_id, follower_user_id)
VALUES
(1, 2), (2, 1), (1, 3), (3, 1),
(1, 4), (4, 1), (1, 5), (5, 1);
```
## 📊 Example Queries
### 1. Update Post Caption
```
sql

UPDATE Posts
SET caption = 'Best pizza ever'
WHERE post_id = 3;
```
### 2. Find Total Number of Likes for All Posts
```
sql

SELECT SUM(num_likes) AS total_likes
FROM (
    SELECT COUNT(Likes.like_id) AS num_likes
    FROM Posts
    LEFT JOIN Likes ON Posts.post_id = Likes.post_id
    GROUP BY Posts.post_id
) AS likes_by_post;
```

### 3. Find Users Who Commented on Post ID = 1
```
sql

SELECT name
FROM Users
WHERE user_id IN (
    SELECT user_id
    FROM Comments
    WHERE post_id = 1
);
```
### 4. Rank Posts by Number of Likes
```
sql

SELECT 
    post_id, 
    num_likes, 
    RANK() OVER (ORDER BY num_likes DESC) AS ranking
FROM (
    SELECT p.post_id, COUNT(l.like_id) AS num_likes
    FROM Posts p
    LEFT JOIN Likes l ON p.post_id = l.post_id
    GROUP BY p.post_id
) AS likes_by_post;
```
### 5. Categorize Posts by Likes
```
sql

SELECT
    post_id,
    CASE
        WHEN num_likes = 0 THEN 'No likes'
        WHEN num_likes < 5 THEN 'Few likes'
        WHEN num_likes < 10 THEN 'Some likes'
        ELSE 'Lots of likes'
    END AS like_category
FROM (
    SELECT p.post_id, COUNT(l.like_id) AS num_likes
    FROM Posts p
    LEFT JOIN Likes l ON p.post_id = l.post_id
    GROUP BY p.post_id
) AS likes_by_post;
```
### 6. Posts Created in the Last Month
```
sql

SELECT *
FROM Posts
WHERE created_at >= DATE_FORMAT(DATE_SUB(CURDATE(), INTERVAL 1 MONTH), '%Y-%m-01')
  AND created_at < DATE_FORMAT(CURDATE(), '%Y-%m-01');
```
### The project also demonstrates the use of:

- **SQL Queries** → WHERE, ORDER BY, GROUP BY, HAVING
- **Aggregation Functions** → COUNT, SUM, AVG, MIN, MAX
- **Advanced SQL** → Subqueries, Window Functions, CTEs, CASE statements
- **Date Functions** → Casting and working with dates

This project helped me practice database design, query optimization, and analytics with SQL, which are core skills for data analysis and backend development.
# 👩‍💻 Author

### Amrutha Nagothi
- 📧 amruthanagothi@gmail.com
- 🌐 [Linkedin](www.linkedin.com/in/amrutha-nagothi-3s)           [GitHub](https://github.com/ammu105)


<p align="center">
  Made with ❤️ by Amrutha  
</p>


