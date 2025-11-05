## Analisis

```sql
PRINT 'Se ejecutarán 3 UPDATES para probar el Trigger de Auditoría...';
UPDATE Usuarios SET Email = 'carlos.developer@dominio.com' WHERE UsuarioID = 1;
UPDATE Usuarios SET NombreUsuario = 'ana_designer_pro' WHERE UsuarioID = 2;
UPDATE Usuarios SET Email = 'pedro.sql.master@basedatos.com', NombreUsuario = 'pedro_el_master_sql' WHERE UsuarioID = 6;
```

## Ver resultado de tabla AuditoriaUsuarios

```sql
SELECT * FROM AuditoriaUsuarios;
```

## Probando la Función

```sql
PRINT '--- Verificando el uso de la Función (fn_ObtenerConteoLikes) ---';
SELECT 
    p.PublicacionID, 
    u.NombreUsuario AS Autor, 
    p.TextoContenido,
    dbo.fn_ObtenerConteoLikes(p.PublicacionID) AS ConteoDeLikes
FROM 
    Publicaciones p
INNER JOIN 
    Usuarios u ON p.UsuarioID = u.UsuarioID
ORDER BY 
    ConteoDeLikes DESC;
```

## Execute del Procedimiento Almacenado

```sql
DECLARE @NuevoID INT;
EXEC dbo.sp_CrearUsuario 'sofia', 'sofia@correo.com', @NuevoID OUTPUT;
SELECT @NuevoID AS Nuevo_id_User;
```