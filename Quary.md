# Python with PostgreSQL using SQLAlchemy

This guide explains how to use **Python** with a **PostgreSQL** database using **SQLAlchemy** ORM, without writing raw SQL queries.

---

## 1. Install Required Packages

```bash
pip install sqlalchemy psycopg2
```

* `sqlalchemy` → ORM (Object Relational Mapper)
* `psycopg2` → PostgreSQL driver for Python

---

## 2. Example: Connecting Python to PostgreSQL

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, sessionmaker

# Connect to PostgreSQL
engine = create_engine("postgresql+psycopg2://username:password@localhost:5432/mydatabase")
Base = declarative_base()

# Define a table
class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)

# Create the table in PostgreSQL
Base.metadata.create_all(engine)

# Create a session
Session = sessionmaker(bind=engine)
session = Session()

# Add a user
new_user = User(name="Alice")
session.add(new_user)
session.commit()

# Query users
for user in session.query(User).all():
    print(user.id, user.name)
```

---

## 3. Key Points

* SQLAlchemy lets you interact with the database using **Python objects**, no SQL required.
* You can perform **CRUD operations** (Create, Read, Update, Delete) easily.
* Works with PostgreSQL, MySQL, SQLite, and other databases.
* Makes code **clean, readable, and maintainable**.

---

## 4. Advantages

1. No raw SQL queries needed.
2. Cross-database compatibility.
3. Easy to scale for bigger projects.
4. Clean and Pythonic syntax.
