# Домашнее задание

Работа с join'ами, статистикой

## Цель

знать и уметь применять различные виды join'ов
строить и анализировать план выполенения запроса
оптимизировать запрос
уметь собирать и анализировать статистику для таблицы

## Описание/Пошаговая инструкция выполнения домашнего задания

```sql
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    department_id INT
);
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL
);
CREATE TABLE projects (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL,
    employee_id INT,
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);

-- Реализовать прямое соединение двух или более таблиц
SELECT e.first_name, e.last_name, d.department_name
   FROM employees e
   INNER JOIN departments d ON e.department_id = d.department_id;

-- Реализовать левостороннее (или правостороннее)
SELECT e.first_name, e.last_name, d.department_name
   FROM employees e
   LEFT JOIN departments d ON e.department_id = d.department_id;

-- Реализовать кросс соединение двух или более таблиц
SELECT e.first_name, e.last_name, d.department_name
   FROM employees e
   CROSS JOIN departments d;

-- Реализовать полное соединение двух или более таблиц
SELECT e.first_name, e.last_name, d.department_name
   FROM employees e
   FULL JOIN departments d ON e.department_id = d.department_id;

-- Реализовать запрос, в котором будут использованы разные типы соединений
SELECT e.first_name, e.last_name, d.department_name, p.project_name
   FROM employees e
   LEFT JOIN departments d ON e.department_id = d.department_id
   INNER JOIN projects p ON e.employee_id = p.employee_id;

-- метрики:
-- 1. Среднее количество проектов на сотрудника
SELECT AVG(project_count) AS avg_projects_per_employee
     FROM (
         SELECT e.employee_id, COUNT(p.project_id) AS project_count
         FROM employees e
         LEFT JOIN projects p ON e.employee_id = p.employee_id
         GROUP BY e.employee_id
     ) AS employee_projects;
-- 2. Процент сотрудников без проектов
SELECT
         COUNT(*) FILTER (WHERE p.project_id IS NULL) * 100.0 / COUNT(*) AS percent_without_projects
     FROM employees e
     LEFT JOIN projects p ON e.employee_id = p.employee_id;
-- 3. Средняя нагрузка отделов (количество проектов на отдел)
SELECT d.department_name, COUNT(p.project_id) AS project_count
     FROM departments d
     LEFT JOIN employees e ON d.department_id = e.department_id
     LEFT JOIN projects p ON e.employee_id = p.employee_id
     GROUP BY d.department_name;
-- Эти метрики помогают оценить распределение проектов среди сотрудников и отделов, а также выявить потенциальные проблемы, такие как отсутствие загруженности у некоторых сотрудников или неравномерное распределение работы между отделами.
```
