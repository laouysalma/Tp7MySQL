## Lab 7 : Transactions, verrouillage et sécurité

## Script SQL :
```sql


-- -Création de la base et de la table
CREATE DATABASE IF NOT EXISTS banque_demo
  CHARACTER SET utf8mb4
  COLLATE utf8mb4_unicode_ci;
USE banque_demo;

CREATE TABLE IF NOT EXISTS compte (
  id INT AUTO_INCREMENT PRIMARY KEY,
  titulaire VARCHAR(100) NOT NULL,
  solde DECIMAL(10,2) NOT NULL DEFAULT 0.00
) ENGINE=InnoDB;

-- 2-Insertion de données
INSERT INTO compte (titulaire, solde) VALUES
('Alice', 1000.00),
('Bob', 500.00);

-- Vérification
SELECT * FROM compte;

-- 3-Transactions avec COMMIT et ROLLBACK
-- Exemple COMMIT
START TRANSACTION;
UPDATE compte SET solde = solde - 200.00 WHERE titulaire='Alice';
UPDATE compte SET solde = solde + 200.00 WHERE titulaire='Bob';
COMMIT;

SELECT * FROM compte;

-- Exemple ROLLBACK
START TRANSACTION;
UPDATE compte SET solde = solde - 2000.00 WHERE titulaire='Alice';
UPDATE compte SET solde = solde + 2000.00 WHERE titulaire='Bob';
ROLLBACK;

SELECT * FROM compte;

-- 4-Changement du niveau d'isolation
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;

START TRANSACTION;
SELECT solde FROM compte WHERE titulaire='Alice'; 
COMMIT;

-- 5-Lock explicite (FOR UPDATE)
START TRANSACTION;
SELECT * FROM compte WHERE titulaire='Alice' FOR UPDATE;
COMMIT;

-- 6-Création d’un utilisateur limité
CREATE USER IF NOT EXISTS 'app_user'@'localhost' IDENTIFIED BY 'P@ssw0rd!';

-- Attribution des droits
GRANT SELECT, INSERT, UPDATE ON banque_demo.compte TO 'app_user'@'localhost';
FLUSH PRIVILEGES;

-- 7-Révocation d’un droit
REVOKE UPDATE ON banque_demo.compte FROM 'app_user'@'localhost';
FLUSH PRIVILEGES;

```
   


## Capture d’écran  
![image alt](https://github.com/laouysalma/Tp7MySQL/blob/main/Ex1.jpg?raw=true)

![image alt](https://github.com/laouysalma/Tp7MySQL/blob/main/Ex2.jpg?raw=true)

![image alt](https://github.com/laouysalma/Tp7MySQL/blob/main/Ex3.jpg?raw=true)
