1. Создание 10 пользователей с паролями

sql

USE master;
GO

-- Создание 10 пользователей с случайными паролями
DECLARE @i INT = 1;
DECLARE @username NVARCHAR(50);
DECLARE @password NVARCHAR(50);
DECLARE @sql NVARCHAR(MAX);

WHILE @i <= 10
BEGIN
    SET @username = 'us' + CAST(@i AS NVARCHAR(50));
    SET @password = LEFT(CONVERT(NVARCHAR(50), NEWID()), 5);

    SET @sql = 'CREATE LOGIN ' + @username + ' WITH PASSWORD = ''' + @password + ''';';
    EXEC sp_executesql @sql;

    -- Создание базы данных
    SET @sql = 'CREATE DATABASE DB' + CAST(@i AS NVARCHAR(50)) + ';';
    EXEC sp_executesql @sql;

    -- Создание пользователя в базе данных
    SET @sql = 'USE DB' + CAST(@i AS NVARCHAR(50)) + ';
               CREATE USER ' + @username + ' FOR LOGIN ' + @username + ';
               ALTER ROLE db_datareader ADD MEMBER ' + @username + ';
               ALTER ROLE db_datawriter ADD MEMBER ' + @username + ';';
    EXEC sp_executesql @sql;

    -- Запрет создания баз данных и управления разрешениями
    SET @sql = 'USE master;
                DENY CREATE DATABASE TO ' + @username + ';
                DENY ALTER ANY LOGIN TO ' + @username + ';
                DENY CONTROL SERVER TO ' + @username + ';';
    EXEC sp_executesql @sql;

    SET @i = @i + 1;
END
GO

Шаг 2: Создание базы данных DB и таблицы Users

sql

-- Создание базы данных DB
CREATE DATABASE DB;
GO

-- Использование базы данных DB
USE DB;
GO

-- Создание таблицы Users для хранения пользователей и паролей
CREATE TABLE Users (
    Id INT IDENTITY PRIMARY KEY,
    UserName NVARCHAR(50) NOT NULL,
    PasswordHash NVARCHAR(255) NOT NULL
);
GO

Шаг 3: Заполнение таблицы Users данными созданных пользователях и паролях

sql

USE DB;
GO

-- Вставка пользователей и паролей
DECLARE @i INT = 1;
DECLARE @username NVARCHAR(50);
DECLARE @password NVARCHAR(50);
DECLARE @sql NVARCHAR(MAX);

WHILE @i <= 10
BEGIN
    SET @username = 'us' + CAST(@i AS NVARCHAR(50));
    SET @password = LEFT(CONVERT(NVARCHAR(50), NEWID()), 5);

    INSERT INTO Users (UserName, PasswordHash)
    VALUES (@username, HASHBYTES('SHA2_256', @password));

    SET @i = @i + 1;
END
GO

Шаг 4: Сценарий для шифрования паролей в таблице Users

sql

USE DB;
GO

-- Сценарий для обновления паролей, если не был выполнен предыдущий шаг
UPDATE Users
SET PasswordHash = HASHBYTES('SHA2_256', PasswordHash);
GO

Шаг 5: Сценарий для отображения данных из таблицы Users с расшифрованными паролями

Обратите внимание, что пароли хранятся в хешированном виде, и расшифровать их невозможно. Вместо этого мы можем проверить введенный пароль против хеша.

sql

USE DB;
GO

-- Проверка пароля пользователя
DECLARE @inputPassword NVARCHAR(50) = 'введите пароль для проверки';
DECLARE @inputHash VARBINARY(255) = HASHBYTES('SHA2_256', @inputPassword);

SELECT UserName
FROM Users
WHERE PasswordHash = @inputHash;
GO

Шаг 6: Сценарий для резервного копирования базы данных DB

sql

-- Сценарий для резервного копирования базы данных DB
USE master;
GO

BACKUP DATABASE DB
TO DISK = 'C:\Backup\DB.bak'
WITH FORMAT,
     MEDIANAME = 'SQLServerBackups',
     NAME = 'Full Backup of DB';
GO

Шаг 7: Сценарий для восстановления базы данных DB

sql

-- Сценарий для восстановления базы данных DB
USE master;
GO

-- Убедитесь, что база данных не используется и выключена
ALTER DATABASE DB SET SINGLE_USER WITH ROLLBACK IMMEDIATE;
GO

-- Восстановление базы данных
RESTORE DATABASE DB
FROM DISK = 'C:\Backup\DB.bak'
WITH FILE = 1,
     NOUNLOAD,
     REPLACE,
     STATS = 5;
GO

-- Возвращение базы данных в многопользовательский режим
ALTER DATABASE DB SET MULTI_USER;
GO
