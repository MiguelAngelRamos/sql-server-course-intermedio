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