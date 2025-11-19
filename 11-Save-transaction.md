## Crear base de datos

```sql

CREATE DATABASE TiendaSave;
GO
USE TiendaSave;
GO

-- 1. Tabla de Productos
CREATE TABLE Productos (
    ProductoID INT PRIMARY KEY IDENTITY(1,1),
    Nombre VARCHAR(100),
    Precio DECIMAL(10, 2),
    Stock INT
);

-- 2. Tabla de Historial (Auditoría)
CREATE TABLE HistorialPrecios (
    HistorialID INT PRIMARY KEY IDENTITY(1,1),
    ProductoID INT,
    PrecioAnterior DECIMAL(10, 2),
    PrecioNuevo DECIMAL(10, 2),
    FechaCambio DATETIME DEFAULT GETDATE()
);

-- Insertamos una Laptop de prueba
INSERT INTO Productos (Nombre, Precio, Stock) 
VALUES ('Laptop Gamer X1', 1500.00, 10);


```

## Sp con Save transaction

```sql
USE TiendaSave;
GO

CREATE OR ALTER PROCEDURE sp_VentaConGarantiaRobusta
AS
BEGIN
    SET NOCOUNT ON;

    -- Definimos el nombre de nuestra bandera de seguridad interna
    DECLARE @NombreSavePoint VARCHAR(32) = 'PuntoSeguro';

    -- INICIO DE LA TRANSACCIÓN GLOBAL
    BEGIN TRANSACTION;

    BEGIN TRY
        -- ========================================================
        -- FASE 1: EL ÉXITO (Queremos que esto se guarde)
        -- ========================================================
        INSERT INTO Productos (Nombre, Precio, Stock)
        VALUES ('Laptop Pro 2025', 2500.00, 5);
        
        PRINT '>> Paso 1: Laptop insertada. (Datos en memoria pendiente de commit)';

        -- ========================================================
        -- 🚩 SAVE POINT (El "Checkpoint")
        -- ========================================================
        -- Si algo falla después de esta línea, intentaremos volver aquí
        -- para no perder la venta de la Laptop.
        SAVE TRANSACTION @NombreSavePoint;
        
        PRINT '>> 🚩 Checkpoint creado. La Laptop está protegida.';


        -- ========================================================
        -- FASE 2: EL RIESGO (Simularemos un error aquí)
        -- ========================================================
        -- Intentamos insertar la Garantía, pero simulamos que falla.
        -- Usamos RAISERROR para saltar directo al CATCH.
        
        PRINT '>> Paso 2: Intentando procesar la Garantía...';
        
        -- Simulamos error de negocio
        RAISERROR('Error: El servicio de garantías no responde.', 16, 1);

        -- (Esta línea nunca se ejecutará por el error de arriba)
        INSERT INTO Productos (Nombre, Precio, Stock) VALUES ('Garantía Extendida', 50.00, 999);
        
        -- Si todo fuera perfecto, confirmaríamos todo aquí.
        COMMIT TRANSACTION;
        PRINT '>> Éxito Total: Se vendió Laptop y Garantía.';
    END TRY
    BEGIN CATCH
        -- ========================================================
        -- MANEJO DE ERRORES INTELIGENTE (Usando XACT_STATE)
        -- ========================================================
        DECLARE @ErrorMsg NVARCHAR(4000) = ERROR_MESSAGE();
        PRINT '>> 💥 Ocurrió un error: ' + @ErrorMsg;

        -- CASO A: La transacción está muerta (Estado -1)
        -- El error fue catastrófico. No se puede salvar nada.
        IF XACT_STATE() = -1
        BEGIN
            PRINT '>> Estado Crítico (-1): La transacción es irrecuperable. Rollback Total.';
            ROLLBACK TRANSACTION; -- Se borra Laptop y Garantía.
        END

        -- CASO B: La transacción sigue viva (Estado 1)
        -- El error fue manejable. Podemos usar el Save Point.
        ELSE IF XACT_STATE() = 1
        BEGIN
            PRINT '>> Estado Saludable (1): El error es parcial. Usando Save Point...';
            
            -- 1. Rebobinamos SOLO hasta la bandera (Deshacemos el intento de garantía)
            ROLLBACK TRANSACTION @NombreSavePoint;
            
            PRINT '>> ↩️ Se deshizo el Paso 2 (Garantía). La Laptop sigue en memoria.';

            -- 2. ¡MUY IMPORTANTE! 
            -- Como el Rollback al SavePoint NO cierra la transacción,
            -- nosotros debemos cerrarla manualmente con COMMIT para 
            -- guardar permanentemente la Laptop.
            COMMIT TRANSACTION;
            
            PRINT '>> ✅ COMMIT FINAL: Se guardó la Laptop exitosamente (Venta Parcial).';
        END
    END CATCH
END;
GO

-- 1. Limpiamos la tabla para ver claro
TRUNCATE TABLE Productos;

-- 2. Ejecutamos el SP Blindado
EXEC sp_VentaConGarantiaRobusta;

-- 3. Verificamos qué quedó en la base de datos
PRINT '--------------------------------';
SELECT * FROM Productos;
```