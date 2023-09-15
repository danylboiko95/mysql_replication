# mysql_replication


### Create table MASTER

`CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    registration_date DATE
);`

### add rows MASTER

`
DELIMITER //
CREATE PROCEDURE Insert100Users()
BEGIN
DECLARE i INT DEFAULT 1;

    WHILE i <= 100 DO
        INSERT INTO users (user_id, username, email, registration_date)
        VALUES
            (i, CONCAT('user', i), CONCAT('user', i, '@example.com'), '2023-09-15');
        SET i = i + 1;
    END WHILE;

END //
DELIMITER ;

CALL Insert100Users();
`

### check replication on slave
<img width="1709" alt="image" src="https://github.com/danylboiko95/mysql_replication/assets/44903844/d74c9ac4-3418-4901-ae69-5a5b491db688">

<img width="1715" alt="image" src="https://github.com/danylboiko95/mysql_replication/assets/44903844/8f033c26-7cc8-40f0-9a36-658b39aa3c0f">


`select * from users;`

### slow query replication

`

DELIMITER //
CREATE PROCEDURE InsertWithSleep100Users()
BEGIN
DECLARE i INT DEFAULT 101;

    WHILE i <= 200 DO
        INSERT INTO users (user_id, username, email, registration_date)
        VALUES
            (i, CONCAT('user', i), CONCAT('user', i, '@example.com'), '2023-09-15');
        SET i = i + 1;

        SELECT SLEEP(5);
    END WHILE;

END //
DELIMITER ;

CALL InsertWithSleep100Users();
`
### drop middle column for slave
<img width="1336" alt="image" src="https://github.com/danylboiko95/mysql_replication/assets/44903844/cfab0b9d-b92d-498a-8afa-f72509355b11">

<img width="1722" alt="image" src="https://github.com/danylboiko95/mysql_replication/assets/44903844/3ea1d9af-62c7-4bdd-ac35-3fe42161a0b7">

<img width="1485" alt="image" src="https://github.com/danylboiko95/mysql_replication/assets/44903844/9e010288-2a78-4e52-bb9a-5e92cc5f21d7">
