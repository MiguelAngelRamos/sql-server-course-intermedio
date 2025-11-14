```sql
ALTER PROCEDURE Med.sp_RegistrarMascota
    -- 1. Parámetros de Entrada: Son los datos que el SP necesita para trabajar
    @NombreMascota NVARCHAR(100),
    @Especie NVARCHAR(50),
    @Raza NVARCHAR(50),
    @FechaNacimiento DATE,
    @PropietarioID INT,
    @NuevoMascotaID INT OUTPUT
AS
BEGIN
    -- (Buena práctica: Evita que SQL devuelva mensajes de "X filas afectadas")
    SET NOCOUNT ON;

    -- Iniciar transacción
    BEGIN TRANSACTION;

    -- 4. Bloque TRY: Intentamos ejecutar la lógica principal
    BEGIN TRY
        -- 2. Validación: verificamos que el PropietarioID exista en la tabla de dueños
        IF NOT EXISTS (SELECT 1 FROM Adm.Propietarios WHERE PropietarioID = @PropietarioID)
        BEGIN
            -- 3. Error Controlado: Si no existe, lanzamos un error claro y salimos.
            RAISERROR('Error de Validación: El PropietarioID %d no existe.', 16, 1, @PropietarioID);
            RETURN;
        END

        -- 5. Lógica de Negocio: La inserción
        INSERT INTO Med.Mascotas
            (NombreMascota, Especie, Raza, FechaNacimiento, PropietarioID)
        VALUES
            (@NombreMascota, @Especie, @Raza, @FechaNacimiento, @PropietarioID);

        -- Obtener el ID generado
        SET @NuevoMascotaID = SCOPE_IDENTITY();

        -- Confirmar la transacción
        COMMIT TRANSACTION;

        -- Retornar mensaje de éxito (opcional)
        PRINT 'Mascota registrada exitosamente. con ID: ' + CAST(@NuevoMascotaID AS VARCHAR(10));

    END TRY
    -- 6. Bloque CATCH: Si algo falla en el TRY (ej. un dato nulo en columna NOT NULL)
    BEGIN CATCH
        -- REVERTIR LA TRANSACCION EN CASO DE ERROR
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;

        -- 7. Manejo del Error: Informamos del error (en un sistema real, lo guardaríamos en un log)
        PRINT 'Ha ocurrido un error inesperado al registrar la mascota.';

        -- Re-lanzamos el error original para que la aplicación que llamó al SP se entere
        -- THROW;
        DECLARE @ErrorMessage NVARCHAR(4000) = ERROR_MESSAGE();
        RAISERROR(@ErrorMessage, 16, 1);
    END CATCH
END

```