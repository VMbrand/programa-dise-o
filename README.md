# DISEÑO: 

CREATE DATABASE Veterinaria;
USE Veterinaria;

-- Tabla de Clientes
CREATE TABLE Clientes (
    cliente_id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    direccion VARCHAR(100),
    telefono VARCHAR(15)
);

-- Tabla de Mascotas
CREATE TABLE Mascotas (
    mascota_id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT,
    nombre VARCHAR(50),
    especie VARCHAR(30),
    raza VARCHAR(50),
    edad INT,
    FOREIGN KEY (cliente_id) REFERENCES Clientes(cliente_id)
);

-- Tabla de Visitas
CREATE TABLE Visitas (
    visita_id INT PRIMARY KEY AUTO_INCREMENT,
    mascota_id INT,
    fecha DATE,
    motivo VARCHAR(100),
    observaciones TEXT,
    FOREIGN KEY (mascota_id) REFERENCES Mascotas(mascota_id)
);
Consultas de Inserción de Datos
sql
Copiar código
-- Insertar datos en Clientes
INSERT INTO Clientes (nombre, direccion, telefono) VALUES 
('Juan Pérez', 'Calle 123', '3001234567'),
('Ana Martínez', 'Avenida 456', '3009876543'),
('Carlos López', 'Carrera 789', '3101122334');

-- Insertar datos en Mascotas
INSERT INTO Mascotas (cliente_id, nombre, especie, raza, edad) VALUES 
(1, 'Rex', 'Perro', 'Pastor Alemán', 5),
(2, 'Luna', 'Gato', 'Siames', 3),
(3, 'Bobby', 'Perro', 'Labrador', 2);

-- Insertar datos en Visitas
INSERT INTO Visitas (mascota_id, fecha, motivo, observaciones) VALUES 
(1, '2024-10-01', 'Consulta general', 'Buen estado general'),
(2, '2024-10-05', 'Vacunación', 'Vacuna triple felina aplicada'),
(3, '2024-10-10', 'Revisión', 'Problemas leves de articulaciones');
Consultas de Selección, Actualización, Edición y Eliminación
Selección de Datos
sql
Copiar código
-- Consultar todos los clientes
SELECT * FROM Clientes;

-- Consultar todas las mascotas
SELECT * FROM Mascotas;

-- Consultar todas las visitas
SELECT * FROM Visitas;

-- Consultar mascotas y sus dueños
SELECT Clientes.nombre AS Cliente, Mascotas.nombre AS Mascota, Mascotas.especie, Mascotas.raza
FROM Mascotas
JOIN Clientes ON Mascotas.cliente_id = Clientes.cliente_id;

-- Consultar visitas de cada mascota
SELECT Mascotas.nombre AS Mascota, Visitas.fecha, Visitas.motivo
FROM Visitas
JOIN Mascotas ON Visitas.mascota_id = Mascotas.mascota_id;
Actualización de Datos
sql
Copiar código
-- Actualizar dirección de un cliente
UPDATE Clientes
SET direccion = 'Calle 456'
WHERE cliente_id = 1;

-- Cambiar la raza de una mascota
UPDATE Mascotas
SET raza = 'Labrador Retriever'
WHERE mascota_id = 3;

-- Modificar observaciones en una visita
UPDATE Visitas
SET observaciones = 'Sin cambios desde la última revisión'
WHERE visita_id = 2;
Edición de Datos
sql
Copiar código
-- Añadir una columna de correo electrónico a la tabla Clientes
ALTER TABLE Clientes ADD COLUMN correo VARCHAR(100);

-- Cambiar el tipo de dato de la columna edad en Mascotas
ALTER TABLE Mascotas MODIFY COLUMN edad SMALLINT;
Eliminación de Datos
sql
Copiar código
-- Eliminar una visita específica
DELETE FROM Visitas WHERE visita_id = 3;

-- Eliminar una mascota de la tabla
DELETE FROM Mascotas WHERE mascota_id = 2;

-- Eliminar un cliente (y todas sus mascotas y visitas, si están configuradas con CASCADE)
DELETE FROM Clientes WHERE cliente_id = 1;
Consultas Adicionales (para llegar a 30)
sql
Copiar código
-- Consultar clientes con más de una mascota
SELECT Clientes.nombre
FROM Clientes
JOIN Mascotas ON Clientes.cliente_id = Mascotas.cliente_id
GROUP BY Clientes.nombre
HAVING COUNT(Mascotas.mascota_id) > 1;

-- Consultar la cantidad de visitas por mascota
SELECT Mascotas.nombre, COUNT(Visitas.visita_id) AS TotalVisitas
FROM Mascotas
LEFT JOIN Visitas ON Mascotas.mascota_id = Visitas.mascota_id
GROUP BY Mascotas.nombre;

-- Buscar clientes por nombre parcial
SELECT * FROM Clientes WHERE nombre LIKE '%ana%';

-- Consultar la mascota más joven
SELECT nombre, MIN(edad) AS Edad
FROM Mascotas;
