## PROCEDIMIENTO MEJORADO CON MANEJO DE TRANSACCIONES Y EXCEPCIONES

```sql
-- PROCEDIMIENTO MEJORADO CON MANEJO DE TRANSACCIONES Y EXCEPCIONES
ALTER PROCEDURE sp_CrearUsuario
    @NombreUsuario VARCHAR(50),
    @Email VARCHAR(100),
    @NuevoUsuarioID INT OUTPUT
AS
BEGIN
    SET NOCOUNT ON;
    
    -- Iniciar transacción
    BEGIN TRANSACTION;
    
    BEGIN TRY
        -- Validaciones de entrada
        IF @NombreUsuario IS NULL OR LTRIM(RTRIM(@NombreUsuario)) = ''
        BEGIN
            RAISERROR('El nombre de usuario no puede estar vacío', 16, 1);
            RETURN;
        END;
        
        IF @Email IS NULL OR LTRIM(RTRIM(@Email)) = ''
        BEGIN
            RAISERROR('El email no puede estar vacío', 16, 1);
            RETURN;
        END;
        
        -- Validar formato de email básico
        IF @Email NOT LIKE '%@%.%'
        BEGIN
            RAISERROR('El formato del email no es válido', 16, 1);
            RETURN;
        END;
        
        -- Verificar que el email no esté duplicado
        IF EXISTS (SELECT 1 FROM Usuarios WHERE Email = @Email)
        BEGIN
            RAISERROR('El email ya está registrado en el sistema', 16, 1);
            RETURN;
        END;
        
        -- Verificar que el nombre de usuario no esté duplicado
        IF EXISTS (SELECT 1 FROM Usuarios WHERE NombreUsuario = @NombreUsuario)
        BEGIN
            RAISERROR('El nombre de usuario ya existe', 16, 1);
            RETURN;
        END;
        
        -- Insertar el nuevo usuario
        INSERT INTO Usuarios (NombreUsuario, Email, FechaRegistro)
        VALUES (@NombreUsuario, @Email, GETDATE());
        
        -- Obtener el ID generado
        SET @NuevoUsuarioID = SCOPE_IDENTITY();
        
        -- Confirmar la transacción
        COMMIT TRANSACTION;
        
        -- Retornar mensaje de éxito (opcional)
        PRINT 'Usuario creado exitosamente con ID: ' + CAST(@NuevoUsuarioID AS VARCHAR(10));
        
    END TRY
    BEGIN CATCH
        -- Revertir la transacción en caso de error
        IF @@TRANCOUNT > 0
            ROLLBACK TRANSACTION;
        
        -- Capturar y lanzar información del error
        DECLARE @ErrorMessage NVARCHAR(4000) = ERROR_MESSAGE();
        DECLARE @ErrorSeverity INT = ERROR_SEVERITY();
        DECLARE @ErrorState INT = ERROR_STATE();
        
        RAISERROR(@ErrorMessage, @ErrorSeverity, @ErrorState);
    END CATCH;
END;


```