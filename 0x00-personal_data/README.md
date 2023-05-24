# 0x00. Personal data

# Concepts
For this project, we expect you to look at this concept:

- [User management](https://intranet.alxswe.com/concepts/558)

# Resources
Read or watch:

- [What Is PII, non-PII, and Personal Data?](https://intranet.alxswe.com/rltoken/jf71oYqiETchcVhPzQVnyg)
- [logging documentation](https://intranet.alxswe.com/rltoken/W2JiHD6cbJY1scJORyLqnw)
- [bcrypt package](https://intranet.alxswe.com/rltoken/41oaQXfzwnF1i-wT8W0vHw)
- [Logging to Files, Setting Levels, and Formatting](https://intranet.alxswe.com/rltoken/XCpI9uvguxlTCsAeRCW6SA)

# Learning Objectives
At the end of this project, you are expected to be able to explain to anyone, without the help of Google:

- Examples of Personally Identifiable Information (PII)
- How to implement a log filter that will obfuscate PII fields
- How to encrypt a password and check the validity of an input password
- How to authenticate to a database using environment variables

# Tasks
0. Regex-ing
- Write a function called filter_datum that returns the log message obfuscated:

Arguments:
fields: a list of strings representing all fields to obfuscate
redaction: a string representing by what the field will be obfuscated
message: a string representing the log line
separator: a string representing by which character is separating all fields in the log line (message)
The function should use a regex to replace occurrences of certain field values.
filter_datum should be less than 5 lines long and use re.sub to perform the substitution with a single regex.

# 1. Log formatter
- Copy the following code into filtered_logger.py.

import logging


class RedactingFormatter(logging.Formatter):
    """ Redacting Formatter class
        """

    REDACTION = "***"
    FORMAT = "[HOLBERTON] %(name)s %(levelname)s %(asctime)-15s: %(message)s"
    SEPARATOR = ";"

    def __init__(self):
        super(RedactingFormatter, self).__init__(self.FORMAT)

    def format(self, record: logging.LogRecord) -> str:
        NotImplementedError
Update the class to accept a list of strings fields constructor argument.

Implement the format method to filter values in incoming log records using filter_datum. Values for fields in fields should be filtered.

DO NOT extrapolate FORMAT manually. The format method should be less than 5 lines long.

bob@dylan:~$ cat main.py
#!/usr/bin/env python3
"""
Main file
"""

import logging
import re

RedactingFormatter = __import__('filtered_logger').RedactingFormatter

message = "name=Bob;email=bob@dylan.com;ssn=000-123-0000;password=bobby2019;"
log_record = logging.LogRecord("my_logger", logging.INFO, None, None, message, None, None)
formatter = RedactingFormatter(fields=("email", "ssn", "password"))
print(formatter.format(log_record))

bob@dylan:~$
bob@dylan:~$ ./main.py
[HOLBERTON] my_logger INFO 2019-11-19 18:24:25,105: name=Bob; email=***; ssn=***; password=***;
bob@dylan:~$

# 2. Create logger
- Use user_data.csv for this task

Implement a get_logger function that takes no arguments and returns a logging.Logger object.

The logger should be named "user_data" and only log up to logging.INFO level. It should not propagate messages to other loggers. It should have a StreamHandler with RedactingFormatter as formatter.

Create a tuple PII_FIELDS constant at the root of the module containing the fields from user_data.csv that are considered PII. PII_FIELDS can contain only 5 fields - choose the right list of fields that can are considered as “important” PIIs or information that you must hide in your logs. Use it to parameterize the formatter.
# 3. Connect to secure database
- Database credentials should NEVER be stored in code or checked into version control. One secure option is to store them as environment variable on the application server.

In this task, you will connect to a secure holberton database to read a users table. The database is protected by a username and password that are set as environment variables on the server named PERSONAL_DATA_DB_USERNAME (set the default as “root”), PERSONAL_DATA_DB_PASSWORD (set the default as an empty string) and PERSONAL_DATA_DB_HOST (set the default as “localhost”).

The database name is stored in PERSONAL_DATA_DB_NAME.

Implement a get_db function that returns a connector to the database (mysql.connector.connection.MySQLConnection object).

Use the os module to obtain credentials from the environment
Use the module mysql-connector-python to connect to the MySQL database (pip3 install mysql-connector-python)

# 4. Read and filter data
- Implement a main function that takes no arguments and returns nothing.

The function will obtain a database connection using get_db and retrieve all rows in the users table and display each row under a filtered format like this:
# 5. Encrypting passwords
- User passwords should NEVER be stored in plain text in a database.

Implement a hash_password function that expects one string argument name password and returns a salted, hashed password, which is a byte string.

Use the bcrypt package to perform the hashing (with hashpw).
# 6. Check valid password
- Implement an is_valid function that expects 2 arguments and returns a boolean.

Arguments:

hashed_password: bytes type
password: string type
Use bcrypt to validate that the provided password matches the hashed password.


# AUTHOR
- [Simanga Mchunu](https://twitter.com/Simacoder)
