# Bcrypt

To generate a new salt

```python
salt = bcrypt.gensalt()
salt = bcrypt.gensalt(round=12)
```

To hash a password with a salt

```python
hashed_password_in_db = bcrypt.hashpw(b'mypassword', salt)
```

To check the hash password

```python
bcrypt.checkpw(b'my_password', hashed_password_in_db)
```

## Algorithm

The format of the hash

```
$2a$12$R9h/cIPz0gi.URNNX3kh2OPST9/PgBkqquzi.Ss7KIUgO2t0jWMUW
\__/\/ \____________________/\_____________________________/
Alg Cost      Salt                        Hash
```