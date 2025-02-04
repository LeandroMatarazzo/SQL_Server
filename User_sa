-- Creazione di un nuovo utente con gli stessi permessi di 'sa'
USE master;
GO

-- Creazione del login
CREATE LOGIN [nuovo_sa] WITH PASSWORD = 'LaTuaPasswordSicura';
GO

-- Assegnazione del ruolo di sysadmin al nuovo login
ALTER SERVER ROLE sysadmin ADD MEMBER [nuovo_sa];
GO

-- Assegnazione dei permessi sui database specificati
DECLARE @databases TABLE (name NVARCHAR(255));
INSERT INTO @databases (name)
VALUES ('ADB_Demo'), ('ADB_MiaAzienda'), ('ADB_System'), ('ADW_Demo'), ('ADW_MiaAzienda'), ('ALUS_DB');

DECLARE @dbName NVARCHAR(255);
DECLARE @sql NVARCHAR(MAX);

DECLARE db_cursor CURSOR FOR
SELECT name FROM @databases;

OPEN db_cursor;
FETCH NEXT FROM db_cursor INTO @dbName;

WHILE @@FETCH_STATUS = 0
BEGIN
    SET @sql = 'USE ' + QUOTENAME(@dbName) + ';
    CREATE USER [nuovo_sa] FOR LOGIN [nuovo_sa];
    ALTER ROLE db_owner ADD MEMBER [nuovo_sa];';
    
    EXEC sp_executesql @sql;
    
    FETCH NEXT FROM db_cursor INTO @dbName;
END

CLOSE db_cursor;
DEALLOCATE db_cursor;
GO
