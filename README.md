# Actividad-1-Desarrollo-de-sistemas-web-II
Codigo SQL Server

USE TiendaSara;


-- 1. Tabla Categorías
CREATE TABLE Categorias (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Descripcion VARCHAR(100) NOT NULL
);


-- 2. Tabla Marcas
CREATE TABLE Marcas (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Descripcion VARCHAR(100) NOT NULL
);


-- 3. Tabla Productos (con llaves foráneas a Categorías y Marcas)
CREATE TABLE Productos (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Descripcion VARCHAR(150) NOT NULL,
    Precio DECIMAL(18,2) NOT NULL,
    Cantidad INT NOT NULL,
    idCategoria INT NOT NULL,
    idMarca INT NOT NULL,
    CONSTRAINT FK_Productos_Categorias FOREIGN KEY (idCategoria) REFERENCES Categorias(Id),
    CONSTRAINT FK_Productos_Marcas FOREIGN KEY (idMarca) REFERENCES Marcas(Id)
);


-- 4. Tabla Carrito
CREATE TABLE Carrito (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    FolioVenta VARCHAR(50) NOT NULL,
    TotalCompra DECIMAL(18,2) NOT NULL,
    Estatus VARCHAR(50) NOT NULL,
    Fecha DATETIME NOT NULL DEFAULT GETDATE()
);


-- 5. Tabla CarritoDetalle (con llaves foráneas a Carrito y Productos)
CREATE TABLE CarritoDetalle (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    IdCarrito INT NOT NULL,
    IdProducto INT NOT NULL,
    Cantidad INT NOT NULL,
    Subtotal DECIMAL(18,2) NOT NULL,
    CONSTRAINT FK_CarritoDetalle_Carrito FOREIGN KEY (IdCarrito) REFERENCES Carrito(Id),
    CONSTRAINT FK_CarritoDetalle_Productos FOREIGN KEY (IdProducto) REFERENCES Productos(Id)
);


INSERT INTO Categorias (Descripcion) VALUES 
('Electrónica'),
('Línea Blanca'),
('Ropa y Calzado'),
('Hogar y Jardín'),
('Deportes');


INSERT INTO Marcas (Descripcion) VALUES 
('Sony'),
('Samsung'),
('Nike'),
('Whirlpool'),
('Adidas');


INSERT INTO Productos (Descripcion, Precio, Cantidad, idCategoria, idMarca) VALUES 
('Pantalla Smart TV 55 pulgadas', 8500.00, 15, 1, 2), -- Categoría 1 (Electrónica), Marca 2 (Samsung)
('Audífonos Inalámbricos Bluetooth', 1200.00, 40, 1, 1), -- Categoría 1 (Electrónica), Marca 1 (Sony)
('Tenis deportivos para correr', 1499.50, 25, 3, 3), -- Categoría 3 (Ropa y Calzado), Marca 3 (Nike)
('Refrigerador Duplex 14 pies', 12500.00, 8, 2, 4), -- Categoría 2 (Línea Blanca), Marca 4 (Whirlpool)
('Playera deportiva running', 499.00, 50, 3, 5); -- Categoría 3 (Ropa y Calzado), Marca 5 (Adidas)


INSERT INTO Carrito (FolioVenta, TotalCompra, Estatus, Fecha) VALUES 
('FOL-2026-001', 9700.00, 'Completado', '2026-06-01 10:30:00'),
('FOL-2026-002', 1499.50, 'Pendiente', '2026-06-02 14:15:00'),
('FOL-2026-003', 12500.00, 'Completado', '2026-06-03 09:00:00'),
('FOL-2026-004', 1699.00, 'Cancelado', '2026-06-04 18:45:00'),
('FOL-2026-005', 8500.00, 'Proceso', '2026-06-05 11:20:00');


INSERT INTO CarritoDetalle (IdCarrito, IdProducto, Cantidad, Subtotal) VALUES 
(1, 1, 1, 8500.00), -- Carrito 1 compra 1 Pantalla Smart TV (Precio: 8500.00)
(1, 2, 1, 1200.00), -- Carrito 1 compra 1 Audífonos Inalámbricos (Precio: 1200.00) -> Total del Carrito: 9700.00
(2, 3, 1, 1499.50), -- Carrito 2 compra 1 Par de tenis deportivos (Precio: 1499.50)
(3, 4, 1, 12500.00), -- Carrito 3 compra 1 Refrigerador Duplex (Precio: 12500.00)
(4, 5, 2, 998.00);  -- Carrito 4 compra 2 Playeras deportivas (499.00 * 2 = 998.00)


SELECT 
    p.Id AS IdProducto,
    p.Descripcion AS Producto,
    p.Precio,
    p.Cantidad AS Stock,
    c.Descripcion AS Categoria,
    m.Descripcion AS Marca
FROM Productos p
INNER JOIN Categorias c ON p.idCategoria = c.Id
INNER JOIN Marcas m ON p.idMarca = m.Id;


SELECT 
    c.Id AS IdCarrito,
    c.FolioVenta,
    c.Fecha,
    c.Estatus,
    p.Descripcion AS Producto,
    cd.Cantidad AS CantidadComprada,
    cd.Subtotal,
    c.TotalCompra AS TotalGeneralCarrito
FROM Carrito c
INNER JOIN CarritoDetalle cd ON c.Id = cd.IdCarrito
INNER JOIN Productos p ON cd.IdProducto = p.Id;


SELECT 
    c.Id AS IdCarrito,
    c.FolioVenta,
    c.Fecha,
    c.Estatus,
    p.Descripcion AS Producto,
    m.Descripcion AS Marca,
    cat.Descripcion AS Categoria,
    cd.Cantidad AS CantidadComprada,
    cd.Subtotal,
    c.TotalCompra AS TotalGeneralCarrito
FROM Carrito c
INNER JOIN CarritoDetalle cd ON c.Id = cd.IdCarrito
INNER JOIN Productos p ON cd.IdProducto = p.Id
INNER JOIN Marcas m ON p.idMarca = m.Id
INNER JOIN Categorias cat ON p.idCategoria = cat.Id;



