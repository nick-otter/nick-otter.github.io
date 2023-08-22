---
layout: post
title:  "Helpful Python Links"
categories: python
---

# Helpful Python Links

## SOLID

- [SOLID Principles: Improve Object-Oriented Design in Python](https://realpython.com/solid-principles-python/)


## Project Structure

- [What is the best project structure for a Python application?](https://stackoverflow.com/questions/193161/what-is-the-best-project-structure-for-a-python-application)
```
Project/
|-- bin/
|   |-- project
|
|-- project/
|   |-- test/
|   |   |-- __init__.py
|   |   |-- test_main.py
|   |   
|   |-- __init__.py
|   |-- main.py
|
|-- setup.py
|-- README
```


## Error Handling

- [Try, Except for API connections](https://stackoverflow.com/questions/16511337/correct-way-to-try-except-using-python-requests-module)
```
url='http://www.google.com/blahblah'

try:
    r = requests.get(url,timeout=3)
    r.raise_for_status()
except requests.exceptions.HTTPError as errh:
    print ("Http Error:",errh)
except requests.exceptions.ConnectionError as errc:
    print ("Error Connecting:",errc)
except requests.exceptions.Timeout as errt:
    print ("Timeout Error:",errt)
except requests.exceptions.RequestException as err:
    print ("OOps: Something Else",err)

Http Error: 404 Client Error: Not Found for url: http://www.google.com/blahblah
```
## __init__.py

- [What is init py for?](https://stackoverflow.com/questions/448271/what-is-init-py-for)
```
database/
    __init__.py
    schema.py
    insertions.py
    queries.py
```
```
# __init__.py
import os

from sqlalchemy.orm import sessionmaker
from sqlalchemy import create_engine

engine = create_engine(os.environ['DATABASE_URL'])
Session = sessionmaker(bind=engine)
```

---