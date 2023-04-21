
# SQL: Joins - Many To Many 





## Table of Content


**1. SQL: Many To Many**
 - Many to Many Exercise.sql 
## Project Description

**Joins-One To Many:**

    CREATE DATABASE tv_series;
![1](https://user-images.githubusercontent.com/128286364/233397748-041b7952-c190-4ea7-9bd4-7b0351a23f06.png)
![2](https://user-images.githubusercontent.com/128286364/233397803-1570180f-aff7-4fab-85cc-b49512eeed61.png)

    CREATE TABLE reviewers (
	    id INT AUTO_INCREMENT PRIMARY KEY,
	    first_name VARCHAR(100),
	    last_name VARCHAR(100)
    );
![3](https://user-images.githubusercontent.com/128286364/233397850-67996eb6-7523-405d-a991-1f05caf7856a.png)
![4](https://user-images.githubusercontent.com/128286364/233397892-c3db4596-bed6-4606-b70d-e25adca30fe8.png)


    CREATE TABLE series(
	    id INT AUTO_INCREMENT PRIMARY KEY,
	    title VARCHAR(100),
	    released_year YEAR(4),
	    genre VARCHAR(100)
    );
![7](https://user-images.githubusercontent.com/128286364/233398052-afc4d141-0aec-4267-a0d1-1595ecf5c765.png)
![8](https://user-images.githubusercontent.com/128286364/233398087-680808a8-9a5a-40e2-ad5a-03e36a76a114.png)


    CREATE TABLE reviews (
        id INT AUTO_INCREMENT PRIMARY KEY,
        rating DECIMAL(2,1),
        series_id INT,
        reviewer_id INT,
        FOREIGN KEY(series_id) REFERENCES series(id),
        FOREIGN KEY(reviewer_id) REFERENCES reviewers(id)
    );
![5](https://user-images.githubusercontent.com/128286364/233398208-d7011a4c-783c-4d9f-b6f8-1558c516ea11.png)
![6](https://user-images.githubusercontent.com/128286364/233398248-02d9ee60-1407-4c8f-95ef-051a93e24cd0.png)


**1. CHALLENGE 1: Join title of the TV series with its ratings**
	 
    SELECT title, rating FROM series	
    JOIN reviews
        ON series.id = reviews.series_id;
 ![9](https://user-images.githubusercontent.com/128286364/233398379-a47458e8-0dc6-46ec-8a7d-9578d8555d56.png)


**2. CHALLENGE 2: Join title with their AVG rating from reviewers**

    SELECT
	    title,
	AVG(rating) AS average_ratings
	FROM series
    JOIN reviews
	    ON series.id = reviews.series_id
    GROUP BY series.id
    ORDER BY average_ratings;
![10](https://user-images.githubusercontent.com/128286364/233398462-66e26879-e3ce-41c2-9481-099426185eb3.png)


**3. CHALLENGE 3: Reviewers and their ratings**

    SELECT
        first_name,
        last_name,
        rating
	FROM reviewers
    INNER JOIN reviews
        ON reviewers.id = reviews.reviewer_id;
![11](https://user-images.githubusercontent.com/128286364/233398563-f0eb00ca-9cfa-47e4-a1cc-099e4d4ff47c.png)


**4. CHALLENGE 4: Unviewed series**

    SELECT 
        title AS unreviewed_series,
        rating
    FROM series 
    LEFT JOIN reviews
        ON series.id = reviews.series_id
    WHERE rating is NULL;
![12](https://user-images.githubusercontent.com/128286364/233709216-74a795fe-fa09-4a9f-9948-f8a67406a7b2.png)


**5. CHALLENGE 5: GENRE AVG RATINGS**

    SELECT
        genre,
        ROUND(AVG(rating), 2) 
        AS average_rating
    FROM series
    JOIN reviews 
        ON series.id = reviews.series_id
    GROUP BY genre
    ORDER BY genre;
![13](https://user-images.githubusercontent.com/128286364/233709261-633a553b-fd21-42ad-bb65-4be58e3eb05f.png)


**6. CHALLENGE 6:**

**a) Reviewer Stats**

    SELECT 
        first_name,
        last_name,
        COUNT(rating) AS Count,
        IFNULL(MIN(rating),0) AS MIN,
        IFNULL(MAX(rating),0) AS MAX,
        IFNULL(AVG(rating), 0) AS Average,
        IF(COUNT(rating) >= 1, 'ACTIVE', 'INACTIVE') AS Status
    FROM reviewers 
    LEFT JOIN reviews
        ON reviewers.id = reviews.reviewer_id
        GROUP BY reviewers.id;
![14](https://user-images.githubusercontent.com/128286364/233709348-797c200f-e94e-4e57-b3b1-cd50c2da4af5.png)


**b)Reviewer Stats With POWER USERS**

    SELECT 
        first_name,
        last_name,
        COUNT(rating) AS Count,
        IFNULL(MIN(rating),0) AS MIN,
        IFNULL(MAX(rating),0) AS MAX,
        IFNULL(AVG(rating), 0) AS Average,
        CASE
            WHEN COUNT(rating) >= 10 THEN 'POWER USER'
            WHEN COUNT(rating) >= 1 THEN 'ACTIVE'
            ELSE 'INACTIVE'
        END AS Status
    FROM reviewers 
    LEFT JOIN reviews
        ON reviewers.id = reviews.reviewer_id
        GROUP BY reviewers.id;
  ![15](https://user-images.githubusercontent.com/128286364/233709408-37445476-f6a9-4a6d-8753-168b7b5f655d.png)
      

**7. CHALLENGE 7: TV series, with their ratings and the Reviewers**

    SELECT 
        title,
        rating,
        CONCAT(first_name, ' ', last_name) AS reviewers
    FROM reviewers
    INNER JOIN reviews
        ON reviewers.id = reviews.reviewer_id
    INNER JOIN series
        ON series.id = reviews.series_id
    ORDER BY title;
![16](https://user-images.githubusercontent.com/128286364/233709662-67bb03c3-b8a2-48eb-a534-6b764bdb7e2d.png)
    
    
## Installation

To run the program

mysq-ctl cli;
