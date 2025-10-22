
```sql
USE TechDataCorpDB;
GO

-- Opcional: asegurar formato de fecha ISO
SET DATEFORMAT ymd;
GO

/* =========================================================
   1) Datos de catálogo: Products
   ========================================================= */
INSERT INTO catalog.Products (ProductCode, ProductName, UnitPrice, IsActive)
VALUES
 ('P-1001', N'Auriculares Inalámbricos Pro',         59990.00, 1),
 ('P-1002', N'Teclado Mecánico RGB',                  74990.00, 1),
 ('P-1003', N'Mouse Gamer 8K DPI',                    39990.00, 1),
 ('P-1004', N'Monitor 27" 144Hz IPS',               289990.00, 1),
 ('P-1005', N'Webcam Full HD con Micrófono',          32990.00, 1),
 ('P-1006', N'Silla Ergonómica Oficina',             159990.00, 1),
 ('P-1007', N'Dock USB-C 8 en 1',                     54990.00, 1),
 ('P-1008', N'Notebook 14" i5 16GB 512GB',           749990.00, 1),
 ('P-1009', N'Disco SSD NVMe 1TB',                    99990.00, 1),
 ('P-1010', N'Parlantes Bluetooth 20W',               45990.00, 1);
GO

/* =========================================================
   2) Datos de clientes: Customers
   ========================================================= */
INSERT INTO sales.Customers (FirstName, LastName, Email, PhoneNumber)
VALUES
 (N'Carolina',  N'Pérez',      N'carolina.perez@example.com',  N'+56 9 6123 4567'),
 (N'Javier',    N'González',   N'javier.gonzalez@example.com', N'+56 9 7123 4567'),
 (N'Mariana',   N'Rojas',      N'mariana.rojas@example.com',   N'+56 9 8123 4567'),
 (N'Felipe',    N'Contreras',  N'felipe.contreras@example.com',N'+56 9 9222 3344'),
 (N'Camila',    N'Muñoz',      N'camila.munoz@example.com',    N'+56 9 9333 4455'),
 (N'Rodrigo',   N'Vega',       N'rodrigo.vega@example.com',    N'+56 9 9444 5566'),
 (N'Paula',     N'Araya',      N'paula.araya@example.com',     N'+56 9 9555 6677'),
 (N'Andrés',    N'Tapia',      N'andres.tapia@example.com',    N'+56 9 9666 7788');
GO

/* =========================================================
   3) Órdenes: Orders
   - Status: N (Nueva), P (Pagada), C (Cancelada)
   - Fechas en 2024-2025 para variedad
   ========================================================= */
INSERT INTO sales.Orders (CustomerId, OrderDate, Status)
VALUES
 (1, '2024-11-15T10:35:00', 'P'),  -- Carolina
 (2, '2024-12-02T16:10:00', 'P'),  -- Javier
 (3, '2025-01-07T09:05:00', 'N'),  -- Mariana
 (4, '2025-01-20T14:22:00', 'P'),  -- Felipe
 (5, '2025-02-03T11:40:00', 'C'),  -- Camila (cancelada)
 (6, '2025-02-18T18:55:00', 'P'),  -- Rodrigo
 (7, '2025-03-05T12:30:00', 'N'),  -- Paula
 (8, '2025-03-21T15:15:00', 'P'),  -- Andrés
 (1, '2025-04-04T10:10:00', 'P'),  -- Carolina (segunda compra)
 (3, '2025-04-18T19:45:00', 'P');  -- Mariana (segunda compra)
GO

/* =========================================================
   4) Ítems de órdenes: OrderItems
   - UnitPrice es histórico: se registra el precio al momento de la compra.
   - Usamos precios actuales del catálogo como referencia.
   ========================================================= */

-- Orden 1 (Carolina, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (1, 1,  1, 59990.00),     -- Auriculares
 (1, 3,  1, 39990.00);     -- Mouse

-- Orden 2 (Javier, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (2, 2,  1, 74990.00),     -- Teclado
 (2, 5,  1, 32990.00),     -- Webcam
 (2, 9,  1, 99990.00);     -- SSD 1TB

-- Orden 3 (Mariana, Nueva)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (3, 4,  1, 289990.00),    -- Monitor
 (3, 7,  1, 54990.00);     -- Dock

-- Orden 4 (Felipe, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (4, 8,  1, 749990.00),    -- Notebook
 (4, 9,  2, 99990.00);     -- SSD x2

-- Orden 5 (Camila, Cancelada) - igual puede tener ítems registrados
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (5, 6,  1, 159990.00);    -- Silla

-- Orden 6 (Rodrigo, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (6, 10, 2, 45990.00),     -- Parlantes x2
 (6, 1,  1, 59990.00);     -- Auriculares

-- Orden 7 (Paula, Nueva)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (7, 2,  1, 74990.00);     -- Teclado

-- Orden 8 (Andrés, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (8, 5,  1, 32990.00),     -- Webcam
 (8, 3,  1, 39990.00),     -- Mouse
 (8, 7,  1, 54990.00);     -- Dock

-- Orden 9 (Carolina, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (9, 4,  1, 289990.00);    -- Monitor

-- Orden 10 (Mariana, Pagada)
INSERT INTO sales.OrderItems (OrderId, ProductId, Quantity, UnitPrice) VALUES
 (10, 8, 1, 749990.00),    -- Notebook
 (10, 1, 1, 59990.00);     -- Auriculares
GO

```