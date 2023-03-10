# Data Security
> Data security refers to the protection of digital information and systems from unauthorized access, use, disclosure, disruption, modification, or destruction. It encompasses a wide range of measures and technologies that are used to safeguard sensitive and confidential information.
## AUTHENTICATION
Authentication is the process of verifying the identity of a user. In the context of data security, it is used to ensure that only authorized users are able to access specific databases or tables. The CREATE USER statement is used to create a new user with a specified password, and the GRANT statement is used to grant that user access to a specific database or table.
> This command is used to create a new user with a specified password
> U normally need to use the master database to do something with users ```use master```
```sql
    USE master;
    CREATE LOGIN login_name
    WITH PASSWORD = 'passwort', CHECK_POLICY = OFF;
```
> Login for a specific user
```sql
    USE myDatabase;
    CREATE USER user_name FOR LOGIN login_name;
```
> This command is used to grant a user access to a specific database or table
```sql
    GRANT SELECT, INSERT, UPDATE, DELETE ON table_name TO user_name;
```
> Object permission assigned to individual table
```sql
    GRANT SELECT ON example_table TO user_name;
```
> Grant a user specified permissions on a schema
```sql
    GRANT UPDATE, INSERT, DELETE ON SCHEMA::Test TO user_name;
```
> Remove specified permissions for schemas from a user
```sql
    REVOKE UPDATE, INSERT, DELETE ON SCHEMA::Test FROM user_name;
```
> Add a user to role
```sql
    ALTER ROLE role_name ADD MEMBER user_name;
```
> Remove user from a role
```sql
    ALTER ROLE role_name DROP MEMBER user_name;
```
## AUTHORIZATION
Authorization is the process of granting or revoking access to specific resources based on a user's role or level of privilege. The REVOKE statement is used to revoke a user's access to a specific database or table.
> Execute commands as a specifc user with specific rules on the database
```sql
    Execute as  User = "user_name"

    Revert
```
> Assign a server role to the login
```sql
    USE master;
    ALTER SERVER ROLE role_name ADD MEMBER login_name;
```
> Revoke a server role from the login
```sql
    ALTER SERVER ROLE role_name DROP MEMBER login_name;
```
> Assign a database role to the user
```sql
    USE mydatabase;
    ALTER ROLE role_name ADD MEMBER user_name;
```
> Revoke a role from the user
```sql
    ALTER ROLE role_name DROP MEMBER user_name;
```
> This command is used to revoke a user's access to a specific database or table
```sql
    REVOKE SELECT, INSERT, UPDATE, DELETE ON table_name FROM user_name;
```

## Predefined Roles
> Creating server logins or database users alone does not give them explicit rights to perform operations on the server or the database. Only the default membership in the public role allows principals to have the permissions assigned to the public role. Roles are to be understood as a collection of different, usually logically related permissions, and serve for simple and clear permission management. SQL Server comes with some predefined roles.

| Principal     | Description                                                                                                                                                                                                                | Example Roles                        |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| Server Login  | Server-level authorization. Typically, there is no direct assignment of permissions, but membership in specific server roles. Login permissions are stored in the master system database                                   | sysadmin, serveradmin, securityadmin |
| Database User | Database-level authorization. By assigning predefined database roles, a simple permission management can be implemented. Users, database roles, and the permissions granted to them are stored in the respective database. | db_datareader, db_datawriter         |

## ROW LEVEL SECURITY
Row level security is a feature that allows you to restrict access to specific rows in a table based on a user's role or level of privilege. The CREATE POLICY statement is used to create a policy that restricts access to specific rows in a table based on a user's role.
> This command is used to create a policy that restricts access to specific rows in a table based on a user's role
```sql
    CREATE POLICY policy_name ON table_name
    FOR SELECT
    WITH CHECK (condition)
```

## DYNAMIC DATA MASKING
Dynamic data masking is a feature that allows you to mask sensitive data in a table. The ALTER TABLE statement is used to add a masking function to a specific column in a table.
> Default
 ```sql
    ALTER TABLE table_name
    ALTER COLUMN example_column ADD MASKED WITH (FUNCTION = 'default()')
```
> Email
```sql
    ALTER TABLE table_name
    ALTER COLUMN example_column ADD MASKED WITH (FUNCTION = 'email()')
```
> Random
 ```sql
    ALTER TABLE table_name
    ALTER COLUMN example_column ADD MASKED WITH (FUNCTION = 'random(1, 99)')
```
> Custom String
```sql
    ALTER TABLE table_name
    ALTER COLUMN example_column ADD MASKED WITH (FUNCTION = 'partial(1,"XXXXXXX",0)')
```
> Drop DDM
```sql
    ALTER TABLE table_name
    ALTER COLUMN example_column DROP MASKED WITH (FUNCTION = 'email()')
 ```

## ENCRYPTION
Encryption is the process of converting plain text into an unreadable format to protect it from unauthorized access. The ALTER TABLE statement is used to add encryption to a specific column in a table.
> This command is used to encrypt a specific column in a table
```sql
    ALTER TABLE table_name
    ALTER COLUMN sensitive_column
    ADD ENCRYPTION (TYPE = Deterministic, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256');
```