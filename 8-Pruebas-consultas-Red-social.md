## Analisis

```sql
PRINT 'Se ejecutarán 3 UPDATES para probar el Trigger de Auditoría...';
UPDATE Usuarios SET Email = 'carlos.developer@dominio.com' WHERE UsuarioID = 1;
UPDATE Usuarios SET NombreUsuario = 'ana_designer_pro' WHERE UsuarioID = 2;
UPDATE Usuarios SET Email = 'pedro.sql.master@basedatos.com', NombreUsuario = 'pedro_el_master_sql' WHERE UsuarioID = 6;
```
