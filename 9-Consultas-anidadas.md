## Consultas Anidadas

```sql

-- Ejemplo 1.1: Usuarios con más publicaciones que el promedio
-- (subconsulta no correlacionada en WHERE)
SELECT 
    u.UsuarioID,
    u.NombreUsuario,
    COUNT(p.PublicacionID) AS TotalPublicaciones
FROM Usuarios u
LEFT JOIN Publicaciones p ON p.UsuarioID = u.UsuarioID
GROUP BY u.UsuarioID, u.NombreUsuario
HAVING COUNT(p.PublicacionID) > (
    SELECT AVG(CAST(CantidadPubs AS FLOAT))
    FROM (
        SELECT COUNT(*) AS CantidadPubs
        FROM Publicaciones
        GROUP BY UsuarioID
    ) AS Promedio
)
ORDER BY TotalPublicaciones DESC;
GO
```

## CTE

```sql

-- Ejemplo 3.1: CTE simple - Resumen de actividad por usuario
WITH ActividadUsuarios AS (
    SELECT 
        u.UsuarioID,
        u.NombreUsuario,
        COUNT(DISTINCT p.PublicacionID) AS TotalPublicaciones,
        COUNT(DISTINCT c.ComentarioID) AS TotalComentarios,
        COUNT(DISTINCT l.PublicacionID) AS TotalLikes
    FROM Usuarios u
    LEFT JOIN Publicaciones p ON p.UsuarioID = u.UsuarioID
    LEFT JOIN Comentarios c ON c.UsuarioID = u.UsuarioID
    LEFT JOIN Likes l ON l.UsuarioID = u.UsuarioID
    GROUP BY u.UsuarioID, u.NombreUsuario
)
SELECT 
    UsuarioID,
    NombreUsuario,
    TotalPublicaciones,
    TotalComentarios,
    TotalLikes,
    (TotalPublicaciones + TotalComentarios + TotalLikes) AS ActividadTotal
FROM ActividadUsuarios
ORDER BY ActividadTotal DESC;
```