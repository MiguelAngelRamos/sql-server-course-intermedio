## Registros para Poblar la Base de Datos "RED SOCIAL"

```sql
/*
================================================================
INSERCIÓN DE DATOS DE EJEMPLO
================================================================
*/

SET NOCOUNT OFF;
GO

PRINT 'Insertando Usuarios...';
INSERT INTO Usuarios (NombreUsuario, Email)
VALUES
('carlos_dev', 'carlos@dominio.com'),
('ana_design', 'ana@dominio.com'),
('maria_data', 'maria@dominio.com'),
('luis_pm', 'luis@dominio.com'),
('sofia_cloud', 'sofia@dominio.com'),
('pedro_sql', 'pedro@dominio.com');
GO

PRINT 'Insertando Seguidores...';
INSERT INTO Seguidores (SeguidorID, SeguidoID)
VALUES
(1, 2), (1, 3), (1, 6),
(2, 1), (2, 3),
(3, 1), (3, 6),
(4, 1), (4, 2), (4, 5),
(6, 1), (6, 3);
GO

PRINT 'Insertando Publicaciones...';
INSERT INTO Publicaciones (UsuarioID, TextoContenido)
VALUES
(1, '¡Hola a todos! Esta es mi primera publicación. #SQL'),
(2, 'Compartiendo mi último proyecto de diseño. ¿Qué opinan? #DiseñoUX'),
(1, 'Estoy practicando Transacciones en SQL Server. ¡COMMIT y ROLLBACK!'),
(3, 'Visualización de datos con Power BI. Los dashboards son clave.'),
(6, '¿Cuál es su función de SQL favorita? La mía es `ROW_NUMBER()`.'),
(5, 'Migración a la nube completada con éxito. #Azure'),
(4, 'Revisión de sprint la próxima semana. ¡A prepararse! #MetodologiaAgil'),
(1, '¿Alguien ha tenido problemas con `NOLOCK`? Es útil pero peligroso.'),
(3, 'Limpiando un dataset de 10 millones de filas. #DataScience'),
(2, 'Buscando inspiración para una nueva paleta de colores. #Diseño'),
(6, 'Recordatorio: `DELETE` sin `WHERE` es una mala idea. Usen transacciones.'),
(1, 'Creando procedimientos almacenados para optimizar consultas.');
GO

PRINT 'Insertando Likes...';
INSERT INTO Likes (PublicacionID, UsuarioID)
VALUES
(1, 2), (1, 3), (1, 4), (1, 5), (1, 6),
(2, 1), (2, 4),
(3, 6), (3, 3),
(4, 1), (4, 2), (4, 6),
(5, 1), (5, 3),
(6, 4), (6, 1),
(8, 6), (8, 3),
(9, 1),
(11, 1), (11, 2), (11, 3), (11, 4), (11, 5),
(12, 6), (12, 3);
GO

PRINT 'Insertando Comentarios...';
INSERT INTO Comentarios (PublicacionID, UsuarioID, TextoComentario)
VALUES
(1, 2, '¡Bienvenido, Carlos!'),
(1, 6, '¡Excelente inicio!'),
(3, 6, '¡Eso es clave! No olvides el `SET XACT_ABORT ON`.'),
(3, 1, '¡Gracias @pedro_sql! Buen consejo.'),
(4, 2, '¡Se ve genial! ¿Qué herramienta usaste?'),
(4, 3, '¡Gracias @ana_design! Fue con Power BI.'),
(5, 1, '¡Buena elección! Yo uso mucho `LEAD` y `LAG`.'),
(5, 3, 'Yo prefiero `COUNT(*) OVER (PARTITION BY ...)`'),
(8, 6, 'Totalmente. Es preferible usar `READ COMMITTED SNAPSHOT ISOLATION`.'),
(11, 4, '¡Uf! ¡Qué miedo! Siempre `BEGIN TRAN` antes.');
GO

PRINT '=======================================================';
PRINT 'INSERCIÓN DE DATOS COMPLETADA.';
PRINT '=======================================================';
GO


```