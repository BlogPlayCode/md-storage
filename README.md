# md-storage

```sql
-- Создание базы данных
CREATE DATABASE IF NOT EXISTS `библиотека`;
USE `библиотека`;

-- 1. Таблица читатель
CREATE TABLE IF NOT EXISTS `читатель` (
  `id_читателя` INT NOT NULL AUTO_INCREMENT,
  `фамилия` VARCHAR(100) NOT NULL,
  `имя` VARCHAR(100) NOT NULL,
  `отчество` VARCHAR(100),
  `дата_регистрации` DATE NOT NULL,
  PRIMARY KEY (`id_читателя`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 2. Таблица профиль_читателя
CREATE TABLE IF NOT EXISTS `профиль_читателя` (
  `id_профиля` INT NOT NULL AUTO_INCREMENT,
  `адрес` VARCHAR(255),
  `телефон` VARCHAR(20),
  `email` VARCHAR(100),
  `дата_рождения` DATE,
  `читатель_id` INT NOT NULL,
  PRIMARY KEY (`id_профиля`),
  FOREIGN KEY (`читатель_id`) REFERENCES `читатель`(`id_читателя`)
    ON DELETE CASCADE
    ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 3. Таблица библиотекарь
CREATE TABLE IF NOT EXISTS `библиотекарь` (
  `id_библиотекаря` INT NOT NULL AUTO_INCREMENT,
  `фамилия` VARCHAR(100) NOT NULL,
  `имя` VARCHAR(100) NOT NULL,
  `отчество` VARCHAR(100),
  `должность` VARCHAR(100) NOT NULL,
  `телефон` VARCHAR(20) NOT NULL,
  PRIMARY KEY (`id_библиотекаря`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 4. Таблица автор
CREATE TABLE IF NOT EXISTS `автор` (
  `id_автора` INT NOT NULL AUTO_INCREMENT,
  `фамилия` VARCHAR(100) NOT NULL,
  `имя` VARCHAR(100) NOT NULL,
  `отчество` VARCHAR(100),
  `страна` VARCHAR(50),
  PRIMARY KEY (`id_автора`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 5. Таблица жанр
CREATE TABLE IF NOT EXISTS `жанр` (
  `id_жанра` INT NOT NULL AUTO_INCREMENT,
  `название_жанра` VARCHAR(100) NOT NULL UNIQUE,
  PRIMARY KEY (`id_жанра`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 6. Таблица книга
CREATE TABLE IF NOT EXISTS `книга` (
  `id_книги` INT NOT NULL AUTO_INCREMENT,
  `название` VARCHAR(255) NOT NULL,
  `год_издания` YEAR NOT NULL,
  `isbn` CHAR(13) NOT NULL UNIQUE,
  `краткое_описание` TEXT,
  PRIMARY KEY (`id_книги`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 7. Таблица экземпляр_книги
CREATE TABLE IF NOT EXISTS `экземпляр_книги` (
  `id_экземпляра` INT NOT NULL AUTO_INCREMENT,
  `книга_id` INT NOT NULL,
  `штрихкод` VARCHAR(50) NOT NULL UNIQUE,
  `состояние` VARCHAR(50) NOT NULL DEFAULT 'хорошее',
  `расположение` VARCHAR(100) NOT NULL,
  PRIMARY KEY (`id_экземпляра`),
  FOREIGN KEY (`книга_id`) REFERENCES `книга`(`id_книги`)
    ON DELETE CASCADE
    ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 8. Таблица выдача
CREATE TABLE IF NOT EXISTS `выдача` (
  `id_выдачи` INT NOT NULL AUTO_INCREMENT,
  `дата_выдачи` DATE NOT NULL,
  `плановая_дата_возврата` DATE NOT NULL,
  `фактическая_дата_возврата` DATE,
  `читатель_id` INT NOT NULL,
  `экземпляр_id` INT NOT NULL,
  PRIMARY KEY (`id_выдачи`),
  FOREIGN KEY (`читатель_id`) REFERENCES `читатель`(`id_читателя`)
    ON DELETE RESTRICT
    ON UPDATE CASCADE,
  FOREIGN KEY (`экземпляр_id`) REFERENCES `экземпляр_книги`(`id_экземпляра`)
    ON DELETE RESTRICT
    ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 9. Таблица штраф
CREATE TABLE IF NOT EXISTS `штраф` (
  `id_штрафа` INT NOT NULL AUTO_INCREMENT,
  `сумма` DECIMAL(8,2) NOT NULL,
  `дата_начисления` DATE NOT NULL,
  `статус_оплаты` VARCHAR(20) NOT NULL DEFAULT 'не оплачен',
  `выдача_id` INT NOT NULL,
  PRIMARY KEY (`id_штрафа`),
  FOREIGN KEY (`выдача_id`) REFERENCES `выдача`(`id_выдачи`)
    ON DELETE CASCADE
    ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 10. Таблица автор_книги (связь many-to-many)
CREATE TABLE IF NOT EXISTS `автор_книги` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `автор_id` INT NOT NULL,
  `книга_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `уникальная_связь` (`автор_id`,`книга_id`),
  FOREIGN KEY (`автор_id`) REFERENCES `автор`(`id_автора`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  FOREIGN KEY (`книга_id`) REFERENCES `книга`(`id_книги`)
    ON DELETE CASCADE
    ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 11. Таблица жанр_книги (связь many-to-many)
CREATE TABLE IF NOT EXISTS `жанр_книги` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `жанр_id` INT NOT NULL,
  `книга_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `уникальная_связь` (`жанр_id`,`книга_id`),
  FOREIGN KEY (`жанр_id`) REFERENCES `жанр`(`id_жанра`)
    ON DELETE CASCADE
    ON UPDATE CASCADE,
  FOREIGN KEY (`книга_id`) REFERENCES `книга`(`id_книги`)
    ON DELETE CASCADE
    ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```
