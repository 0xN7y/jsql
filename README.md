
jsql is a powerful tool and can be used as python library to create, seed, drop, and interact with SQL databases using clean JSON or YAML configurations or Python dictionaries directly.

---

##  Features

-  Use as a CLI or import as a Python module
-  Secure, parameterized SQL (via SQLAlchemy)
-  Supports SQLite and MySQL
-  JSON / YAML input or native Python dicts
-  Fast batch inserts
- Great for automation, and data workflows

---

## Installation

```sh
pip3 install requirements.txt
```

## CLI Usage

 Create Tables
```sh
python3 jsql.py migrate config.yaml
```
Drop existing tables first
```sh
python3 jsql.py migrate config.yaml --drop
```
Insert Data
```sh
python3 jsql_plus.py seed config.yaml
```
Disable batching (insert one row at a time):
```sh
python3 jsql.py seed config.yaml --no-batch
```sh
 Drop Tables
```sh
python3 jsq.py drop config.yaml
```


## Python Library Usage (with Dicts)

Import and use the model directly in your Python code

```
# include jsql file in same directory 
from jsql import SQLAlchemyManager
```

# SQLite example
```py
manager = SQLAlchemyManager("sqlite:///mydb.sqlite3")

data = {
    "tables": {
        "users": {
            "schema": {
                "name": "TEXT",
                "age": "INTEGER"
            },
            "rows": [
                { "name": "user1", "age": 25 },
                { "name": "user2", "age": 22 }
            ]
        }
    }
}

# CREATE TABLE
manager.run_dict(data, mode="create")

# INSERT DATA
manager.run_dict(data, mode="insert")

# DROP TABLE
manager.run_dict(data, mode="drop")
```

Also supports MySQL
```py
manager = SQLAlchemyManager("mysql+pymysql://user:pass@localhost/dbname")
```

# Documentaion Dict / JSON / YAML Format Guide
jsql expects your data (whether a .json file, .yaml file, or dict in Python) to follow a specific structure. This structure defines
  -Database connection URL
  -Tables and their columns
  -Rows to insert

 Python dict Example
```py
config = {
    "url": "sqlite:///mydb.sqlite3",  # or mysql+pymysql://user:pass@host/db
    "tables": {
        "users": {
            "schema": {
                "id": "INTEGER",
                "name": "TEXT",
                "email": "TEXT"
            },
            "rows": [
                { "id": 1, "name": "user1", "email": "user1@user.com" },
                { "id": 2, "name": "user2", "email": "user2@user2.com" }
            ]
        },
        "logs": {
            "schema": {
                "event": "TEXT",
                "created_at": "TEXT"
            },
            "rows": []
        }
    }
}
```

You can pass this directly to
```py
manager.run_dict(config, mode="create")
manager.run_dict(config, mode="insert")
manager.run_dict(config, mode="drop")
```

JSON File Example

Save as example.json:
```json
{
  "url": "sqlite:///mydb.sqlite3",
  "tables": {
    "products": {
      "schema": {
        "id": "INTEGER",
        "title": "TEXT",
        "price": "INTEGER"
      },
      "rows": [
        { "id": 1, "title": "pc", "price": 1000 },
        { "id": 2, "title": "mouse", "price": 20 }
      ]
    }
  }
}
```
Use with CLI
```sh
python jsq.py migrate example.json
python jsq.py seed example.json
```

