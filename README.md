# mysql_replication


### create table MASTER

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
