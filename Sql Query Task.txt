CREATE TABLE employee (
    emp_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    mgr_id INT
);

SHOW Tables;

DESCRIBE employee;

INSERT INTO employee (emp_id, first_name, last_name, mgr_id) VALUES
(101, 'Aarav', 'Nair', NULL),         
(102, 'Diya', 'Sharma', 101),          
(103, 'Rishi', 'Kumar', 101),         
(104, 'Isha', 'Patel', 102),          
(105, 'Vivek', 'Singh', 102),         
(106, 'Tara', 'Ghosh', 103),           
(107, 'Aditi', 'Chopra', 103),         
(108, 'Karan', 'Mehta', 104),          
(109, 'Neel', 'Rai', 104),             
(110, 'Sonal', 'Jadhav', 105);         
           

SELECT * FROM employee;


--Ques 1 --> get all employees under each manager 

SELECT 
    m.emp_id AS manager_id,
    CONCAT(m.first_name, ' ', m.last_name) AS manager_name,
    e.emp_id AS employee_id,
    CONCAT(e.first_name, ' ', e.last_name) AS employee_name
FROM 
    employee e
JOIN 
    employee m ON e.mgr_id = m.emp_id
ORDER BY 
    manager_id;

--Ques 2 --> how many employees are there in under manager alice or any name

SELECT COUNT(*) AS employee_count
FROM employee e
JOIN employee m ON e.mgr_id = m.emp_id
WHERE CONCAT(m.first_name, ' ', m.last_name) = 'Anjali Sharma';

--Ques 3 --> Get all manager details 

SELECT DISTINCT 
    m.emp_id,
    m.first_name,
    m.last_name
FROM 
    employee m
WHERE 
    m.emp_id IN (SELECT DISTINCT e.mgr_id FROM employee e WHERE e.mgr_id IS NOT NULL);

--Ques 4 --> Find any employee who hasn't assigned any manager 

SELECT 
    emp_id,
    first_name,
    last_name
FROM 
    employee
WHERE 
    mgr_id IS NULL;


--Ques 5 --> Write a function to get a full name(first_name+last_name)

DELIMITER $$

CREATE FUNCTION get_full_name(first_name VARCHAR(50), last_name VARCHAR(50))
RETURNS VARCHAR(100)
DETERMINISTIC
BEGIN
    RETURN CONCAT(first_name, ' ', last_name);
END$$

DELIMITER ;


SELECT 
    emp_id,
    get_full_name(first_name, last_name) AS full_name
FROM 
    employee;







