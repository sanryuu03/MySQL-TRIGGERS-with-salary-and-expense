This is MySQL: TRIGGERS with salary and expense.

# SQL Dummy Data

- create table pekerja

      CREATE TABLE pekerja(
      id INT NOT NULL AUTO_INCREMENT,
      first_name VARCHAR(255) NOT NULL,
      last_name VARCHAR(255) NOT NULL,
      hourly_pay INT NOT NULL,
      salary INT,
      JOB VARCHAR(255) NOT NULL,
      PRIMARY KEY(id)
      ) ENGINE = INNODB;

- insert data ke table pekerja

      INSERT INTO pekerja (first_name, last_name, hourly_pay, salary, JOB)
      VALUES ("san", "FE", 50000, NULL, "FE"),
             ("san", "BE", 100000, NULL, "BE"),
             ("san", "ryuu", 200000, NULL, "FS");
      SELECT * FROM pekerja;

      atau 1 data

      INSERT INTO pekerja (first_name, last_name, hourly_pay, salary, JOB)
      VALUES ("san", "ryuu", 200000, NULL, "FS");
      SELECT * FROM pekerja;

- update data ke table pekerja

      UPDATE pekerja
      SET hourly_pay = 51000
      WHERE id = 1;
      SELECT * FROM pekerja;

      atau update all data

      UPDATE pekerja
      SET hourly_pay = hourly_pay + 1;
      SELECT * FROM pekerja;

- delete data ke table pekerja

      DELETE FROM pekerja
      WHERE id = 3;
      SELECT * FROM pekerja;

- create table expenses

      CREATE TABLE expenses(
      id INT NOT NULL AUTO_INCREMENT,
      expense_name VARCHAR(255) NOT NULL,
      expense_total DECIMAL(10, 2) NOT NULL,
      PRIMARY KEY(id)
      ) ENGINE = INNODB;

- insert data ke table expenses

      INSERT INTO expenses (expense_name, expense_total)
      VALUES ("gaji", 0),
             ("supplies", 0),
             ("taxes", 0);
      SELECT * FROM expenses;

- update data ke table expenses

      UPDATE expenses
      SET expense_total = (SELECT SUM(salary) FROM pekerja)
      WHERE expense_name = "gaji";
      SELECT * FROM expenses;

# SQL TRIGGER

- CREATE TRIGGER before_hourly_pay_update

      CREATE TRIGGER before_hourly_pay_update
      BEFORE UPDATE ON pekerja
      FOR EACH ROW
      SET NEW.salary = (NEW.hourly_pay * 160);

- CREATE TRIGGER before_hourly_pay_insert

      CREATE TRIGGER before_hourly_pay_insert
      BEFORE INSERT ON pekerja
      FOR EACH ROW
      SET NEW.salary = (NEW.hourly_pay * 160);

- CREATE TRIGGER after_salary_delete

      CREATE TRIGGER after_salary_delete
      AFTER DELETE ON pekerja
      FOR EACH ROW
      UPDATE expenses
      SET expense_total = expense_total - OLD.salary
      WHERE expense_name = "gaji";

- CREATE TRIGGER after_salary_insert

      CREATE TRIGGER after_salary_insert
      AFTER INSERT ON pekerja
      FOR EACH ROW
      UPDATE expenses
      SET expense_total = expense_total + NEW.salary
      WHERE expense_name = "gaji";

- CREATE TRIGGER after_salary_update

      CREATE TRIGGER after_salary_update
      AFTER UPDATE ON pekerja
      FOR EACH ROW
      UPDATE expenses
      SET expense_total = expense_total + (NEW.salary - OLD.salary)
      WHERE expense_name = "gaji";

- SHOW TRIGGER

      SHOW TRIGGERS;

## Referensi

- [MySQL: TRIGGERS](https://youtu.be/jVbj72YO-8s)
- 160 = 40 jam/minggu x 4 minggu(1 bulan)
- 2080 = 40 jam/minggu x 52 minggu(1 tahun)

## Donasi

- jika kalian suka dengan projek saya dan ingin support saya, bisa donasi via transfer
  - Jago/Jago Syariah bank digital 5055-6459-9169
