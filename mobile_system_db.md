``` 
CREATE TABLE clients (
    client_id int IDENTITY(1, 1) NOT NULL PRIMARY KEY,
	name nvarchar(20),
	birth_date date
);
```
``` 
CREATE TABLE accounts (
  account_id int IDENTITY(1, 1) NOT NULL PRIMARY KEY,
  client_id int FOREIGN KEY REFERENCES clients(client_id),
  balance decimal(18, 2)
);
```
``` 
CREATE TABLE sim_cards (
  sc_id int IDENTITY(1, 1) NOT NULL PRIMARY KEY,
  account_id int NOT NULL FOREIGN KEY REFERENCES accounts(account_id),
  imsi nvarchar(50) UNIQUE
);
```
``` 
INSERT INTO clients (name, birth_date) VALUES
  ('Jon Snow', '1999-09-12'),
  ('Brienne of Tarth', '1987-03-03'),
  ('Tyrion Lannister', '1886-11-26'),
  ('Daenerys Targaryen', '1999-05-01'),
  ('Tormund Giantsbane', '1965-07-02'),
  ('Grey Worm', '1989-05-25'),
  ('Lyanna Mormont', '2001-08-23'),
  ('Arya Stark', '2003-04-04'),
  ('Bran Stark', '1999-09-14'),
  ('Hodor', '1899-01-01');
```
``` status
10 rows affected
```
``` 
INSERT INTO accounts (client_id, balance) VALUES
  (1, 12.23),
  (2, 28.87),
  (3, 3.97),
  (4, 122.20),
  (5, 7.68),
  (6, 9.13),
  (7, 144.78),
  (8, 5.31),
  (9, 2.30),
  (10, 0.96)
;
```
``` status
10 rows affected
```
``` 
INSERT INTO sim_cards (account_id, imsi) VALUES
  (1, '313460000000001'),
  (1, '313460000000002'),
  (1, '313460000000003'),
  (2, '313460000000004'),
  (3, '313460000000005'),
  (3, '313460000000006'),
  (4, '313460000000007'),
  (4, '313460000000008'),
  (4, '313460000000009'),
  (4, '313460000000010'),
  (5, '313460000000011'),
  (5, '313460000000012'),
  (5, '313460000000013'),
  (6, '313460000000014'),
  (7, '313460000000015'),
  (8, '313460000000016'),
  (9, '313460000000017'),
  (9, '313460000000018'),
  (9, '313460000000019'),
  (10, '313460000000020')
;
```
``` status
20 rows affected
```
``` 
SELECT client_id, sum(balance) as balance
FROM accounts
WHERE balance > 100
GROUP BY client_id
```
| client\_id | balance |
| ---------:|-------:|
| 4 | 122.20 |
| 7 | 144.78 |

``` 
SELECT clients.name, acccounts_sim_cards.sc_amount
FROM clients
RIGHT JOIN (
SELECT DISTINCT(accounts.client_id), COUNT(sim_cards.sc_id) as sc_amount
FROM accounts
LEFT JOIN sim_cards
ON accounts.account_id = sim_cards.account_id
GROUP BY accounts.client_id
HAVING COUNT(sim_cards.sc_id) >= 3
) AS acccounts_sim_cards
on clients.client_id = acccounts_sim_cards.client_id
```
| name | sc\_amount |
| :----|---------:|
| Jon Snow | 3 |
| Daenerys Targaryen | 4 |
| Tormund Giantsbane | 3 |
| Bran Stark | 3 |

``` 
SELECT name, birth_date FROM clients WHERE MONTH(GETDATE()) + 1 = MONTH(birth_date)
```
| name | birth\_date |
| :----|:----------|
| Tyrion Lannister | 1886-11-26 |
