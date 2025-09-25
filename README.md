USE [clasesql]
GO

-- Consulta de tabla votos: mostrar todos los registros y columnas

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL]
      ,[Votos]
      ,[Fecha_Publicación]
  FROM dbo.votos;


-- Mostrar solo registros con menos de 150000 votos, renombrando Distrito Federal a Distrito Electoral

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL] AS DISTRITO_ELECTORAL
      ,[Votos]
      ,[Fecha_Publicación]
  FROM Votos

  WHERE votos < 150000;

-- Mostrar solo registros de entidad Durango

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL] AS DISTRITO_ELECTORAL
      ,[Votos]
      ,[Fecha_Publicación]
  FROM [dbo].[votos]

  WHERE ENTIDAD = 'DURANGO';

-- Mostrar solo registros de fechas anteriores o iguales a 2024/06/03

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL] AS DISTRITO_ELECTORAL
      ,[Votos]
      ,[Fecha_Publicación]
  FROM [dbo].[votos]

  WHERE Fecha_Publicación <= '2024-06-04';

-- Mostrar solo registros de fechas anteriores o iguales a 2024/06/08, que sean entidad Morelos y votos menores a 200.000

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL] AS DISTRITO_ELECTORAL
      ,[Votos]
      ,[Fecha_Publicación]
  FROM [dbo].[votos]

  WHERE Fecha_Publicación <= '2024-06-08'
  AND ENTIDAD = 'MORELOS'
  AND Votos < 200000;

-- Mostrar solo registros de fechas posteriores o iguales a 2024/06/02 o de entidad Tabasco, aunque su fecha sea anterior

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL] AS DISTRITO_ELECTORAL
      ,[Votos]
      ,[Fecha_Publicación]
  FROM [dbo].[votos]

  WHERE Fecha_Publicación >= '2024-06-02'
  OR ENTIDAD = 'TABASCO';

-- De la tabla votos vemos solo registros con las entidades de 'Sinaloa', 'Hidalgo' y 'Colima'

SELECT [ID_ENTIDAD]
      ,[ENTIDAD]
      ,[DISTRITO_FEDERAL] AS DISTRITO_ELECTORAL
      ,[Votos]
      ,[Fecha_Publicación]
  FROM dbo.votos

  WHERE ENTIDAD IN ('SINALOA', 'HIDALGO', 'COLIMA');

-- SELECT DISTINCT Selecciona valores distintos de la columna Fecha Publicación

SELECT DISTINCT Fecha_Publicación
  FROM dbo.votos;

-- Entidades Aguascalientes, Coahuila y Guanajuto con votos en las fechas 2024/06/05 y 2024/06/06

SELECT *
FROM dbo.votos
WHERE ENTIDAD IN ('AGUASCALIENTES', 'COAHUILA', 'GUANAJUATO')
AND Fecha_Publicación >= '2024-06-05'
AND Fecha_Publicación <= '2024-06-06';

-- USANDO OPERADOR BETWEEN

-- Cantidad de votos de las entidades Aguascalientes, Coahuila y Guanajuto entre las fechas 2024/06/05 y 2024/06/06

SELECT *
FROM dbo.votos
WHERE ENTIDAD IN ('AGUASCALIENTES', 'COAHUILA', 'GUANAJUATO')
AND Fecha_Publicación BETWEEN '2024-06-05' AND '2024-06-06';

-- Entidades Aguascalientes, Coahuila y Guanajuto con votos entre 40.000 y 200.000

SELECT *
FROM dbo.votos
WHERE ENTIDAD IN ('AGUASCALIENTES', 'COAHUILA', 'GUANAJUATO')
AND Votos BETWEEN 40000 AND 200000;

-- Registros de la tabla votos pero mostrando solo las entidades entre Aguascalientes y Coahuila en orden alfabético

SELECT *
FROM dbo.votos
WHERE ENTIDAD BETWEEN 'AGUASCALIENTES' AND 'COAHUILA'
ORDER BY ENTIDAD ASC;

--- Coincidencias cercanas con LIKE

--- OPERADOR LIKE -----

-- Registros de la columna Distrito Federal que contengan la palabra 'Santiago'

SELECT *
FROM dbo.votos
WHERE DISTRITO_FEDERAL LIKE '%SANTIAGO%';

-- Registros de la columna Distrito Federal que contengan la palabra 'San' al principio

SELECT *
FROM dbo.votos
WHERE DISTRITO_FEDERAL LIKE 'SAN%';

-- Registros de la columna Distrito Federal que contengan la palabra 'paz' al final

SELECT *
FROM dbo.votos
WHERE DISTRITO_FEDERAL LIKE '%PAZ';

--- Manejo de datos nulos

-- Registros de las entidades Aguascalientes, Campeche, Ciudad de México, Durango y Guanajuato que no tengan Distrito Federal renombrando la columna como 'Colonia'

SELECT ENTIDAD, DISTRITO_FEDERAL AS COLONIA, Votos
FROM dbo.votos
WHERE ENTIDAD IN ('AGUASCALIENTES', 'CAMPECHE', 'CIUDAD DE MEXICO', 'DURANGO', 'GUANAJUATO')
AND DISTRITO_FEDERAL IS NULL;

-- Registros de las entidades Aguascalientes, Campeche, Ciudad de México, Durango y Guanajuato
-- que tengan Distrito Federal renombrando la columna como 'Colonia'

SELECT ENTIDAD, DISTRITO_FEDERAL AS COLONIA, Votos
FROM dbo.votos
WHERE ENTIDAD IN ('AGUASCALIENTES', 'CAMPECHE', 'CIUDAD DE MEXICO', 'DURANGO', 'GUANAJUATO')
AND DISTRITO_FEDERAL IS NOT NULL;

-- Filtrar entre las entidades Aguascalientes, Campeche, Ciudad de México, Durango y Guanajuato
-- que no tengan distrito y renombrar los nulos como 'Distrito sin nombre'

SELECT ENTIDAD, COALESCE(DISTRITO_FEDERAL, 'DISTRITO SIN NOMBRE') AS COLONIA, Votos
FROM dbo.votos
WHERE ENTIDAD IN ('AGUASCALIENTES', 'CAMPECHE', 'CIUDAD DE MEXICO', 'DURANGO', 'GUANAJUATO')
AND DISTRITO_FEDERAL IS NULL;

-- Selecciona todos los registros de la tabla votos y los ordena alfabéticamente en la columna ENTIDAD

SELECT *
FROM dbo.votos
ORDER BY ENTIDAD;

-- Selecciona todos los registros de la tabla votos y ordena la columna ENTIDAD alfabéticamente de manera ascendente (A-Z)

SELECT *
FROM dbo.votos
ORDER BY ENTIDAD ASC;

-- Selecciona todos los registros de la tabla votos y ordena la columna ENTIDAD alfabéticamente de manera descendente (Z-A)

SELECT *
FROM dbo.votos
ORDER BY ENTIDAD DESC;

-- Selecciona todos los registros de la tabla votos y ordena la columna DISTRITO_FEDERAL alfabéticamente
-- Al final la columna ENTIDAD ordenada alfabéticamente con los nulos

SELECT *
FROM dbo.votos
ORDER BY CASE WHEN DISTRITO_FEDERAL IS NULL THEN 1 ELSE 0 END, DISTRITO_FEDERAL ASC, ENTIDAD ASC;

-- Selecciona todos los registros de la tabla votos y ordena las columnas Fecha_Publicación y ENTIDAD
-- Columna Fecha_Publicación de la más antigua a la más reciente (ASC)
-- Columna ENTIDAD alfabéticamente (A-Z)

SELECT *
FROM dbo.votos
ORDER BY Fecha_Publicación ASC, ENTIDAD ASC;

-- Selecciona los 10 primeros registros de la tabla votos y ordena las columnas Entidad y Distrito Federal alfabéticamente

SELECT *
FROM dbo.votos
ORDER BY ENTIDAD, DISTRITO_FEDERAL
OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;

--- Uso de COUNT

-- Cuenta la cantidad de filas de la tabla votos

SELECT COUNT (*) AS Filas
FROM dbo.votos;

-- Cantidad de filas de la columna DISTRITO_FEDERAL que no sean NULL

SELECT COUNT (DISTRITO_FEDERAL) AS Filas_Con_Distritos
FROM dbo.votos;

-- Cantidad de filas totales de la tabla votos renombrando la columna como 'BOLETAS_TOTALES'

SELECT COUNT (*) AS BOLETAS_TOTALES
FROM dbo.votos;

-- Cantidad de boletas totales pero solo de la entidad Aguascalientes

SELECT COUNT(Votos) AS Boletas_Totales_Aguascalientes
FROM dbo.votos
WHERE ENTIDAD = 'AGUASCALIENTES';

-- Cantidad de boletas totales donde la columna DISTRITO_FEDERAL es NULL

SELECT COUNT(Votos) AS Boletas_Totales_Distritos_Sin_Nombre
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL;

-- Cantidad de boletas totales de las entidades Aguascalientes, Baja California y Campeche que tengan como Distrito Federal NULL

SELECT COUNT(*) AS Boletas_Totales_Distritos_Sin_Nombre
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL
AND ENTIDAD IN('AGUASCALIENTES', 'BAJA CALIFORNIA', 'CAMPECHE');


-- Uso de SUM

-- Suma del total de votos

SELECT SUM(Votos) AS Votos_Totales
FROM dbo.votos;

-- Suma total de votos en la entidad Aguascalientes

SELECT SUM(Votos) AS Votos_Totales_Aguascalientes
FROM dbo.votos
WHERE ENTIDAD = 'AGUASCALIENTES';

-- Suma total de votos donde los valores son nulos en la columna DISTRITO_FEDERAL

SELECT SUM(Votos) AS Votos_Totales_Sin_Distrito
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL;

-- Suma total de votos en las entidades Aguascalientes, Chihuahua y Morelos que tienen valores nulos en la columna DISTRITO_FEDERAL

SELECT SUM(Votos) AS Votos_Totales_Sin_Distrito
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL
AND ENTIDAD IN ('AGUASCALIENTES', 'CHIHUAHUA', 'MORELOS');


--- Uso de AVG

-- Promedio del total de votos

SELECT AVG(Votos) AS Promedios_Votos_Totales
FROM dbo.votos;

-- Promedio total de votos que no tienen distrito (valores nulos en la columna DISTRITO_FEDERAL)

SELECT AVG(Votos) AS Promedio_Votos_Totales_Sin_Distrito
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL;

-- Promedio total de votos de las entidades Aguascalientes, Baja California y Morelos que no tienen distrito

SELECT AVG(Votos) AS Promedio_Votos_Totales_Sin_Distrito
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL
AND ENTIDAD IN ('AGUASCALIENTES', 'BAJA CALIFORNIA', 'MORELOS');


--- Uso de MIN/MAX

-- Obtiene la primera y la última palabra de la columna DISTRITO_FEDERAL según el orden alfabético (A-Z)

SELECT MIN(DISTRITO_FEDERAL) AS Palabra_mínima, MAX(DISTRITO_FEDERAL) AS Palabra_máxima
FROM dbo.votos;

-- Registros del valor mínimo y máximo de la columna Votos

SELECT MIN(Votos) AS Votos_Mínimos , MAX(Votos) AS Votos_Máximos
FROM dbo.votos;

-- Registros del valor mínimo y máximo de votos a partir del 05/06/2024 (incluido)

SELECT MIN(Votos) AS Mínimos_Votos_Después_del_5_de_junio, MAX(Votos) AS Máximos_Votos_Después_del_5_de_junio
FROM dbo.votos
WHERE Fecha_Publicación >= '2024/06/05';

-- Valores mínimo y máximo de la columna Votos en las entidades donde en DISTRITO_FEDERAL es nulo
-- Reemplazando los valores nulos de DISTRITO_FEDERAL a 'Sin Distrito'

SELECT ENTIDAD, COALESCE(DISTRITO_FEDERAL, 'Sin Distrito') AS Distrito_Federal ,MIN(Votos) AS Mínimos_Votos_Sin_Nombre_De_Distrito, MAX(Votos) AS Máximos_Votos_Sin_Nombre_De_Distrito
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL
GROUP BY ENTIDAD, COALESCE(DISTRITO_FEDERAL, 'Sin Distrito');

-- Valores mínimo y máximo de Votos en las entidades Aguascalientes, Chihuahua y Morelos donde en DISTRITO_FEDERAL es nulo

SELECT ENTIDAD, MIN(Votos) AS Mínimos_Votos_Sin_Nombre_De_Distrito, MAX(Votos) AS Máximos_Votos_Sin_Nombre_De_Distrito
FROM dbo.votos
WHERE DISTRITO_FEDERAL IS NULL
AND ENTIDAD IN ('AGUASCALIENTES', 'CHIHUAHUA', 'MORELOS')
GROUP BY ENTIDAD;

-- Agrupación de la columna ENTIDAD con el total, promedio, mínimo y maximo de votos por entidad

SELECT ENTIDAD , SUM(Votos) AS Total_Votos, AVG(Votos) AS Promedio_Votos, MIN(Votos) AS Mínimos_Votos, MAX(Votos) AS Máximos_Votos
FROM dbo.votos
GROUP BY ENTIDAD;

-- Total, promedio, mínimo y máximo de votos por entidad, filtrando de la columna Votos solo valores superiores a 1.500.000

SELECT ENTIDAD , SUM(Votos) AS Suma_Votos, AVG(Votos) AS Promedio_Votos, MIN(Votos) AS Mínimo_Votos, MAX(Votos) AS Máximo_Votos
FROM Votos
GROUP BY ENTIDAD
HAVING SUM(Votos) >1500000;

-- Total, promedio, mínimo y máximo de votos por entidad y distritos
-- mostrando solo los registros con valores mínimos de votos mayor a 150.000 y excluyendo Ciudad de México

SELECT ENTIDAD, DISTRITO_FEDERAL, SUM(Votos) AS Suma_Votos, AVG(Votos) AS Promedio_Votos, MIN(Votos) AS Mínimo_Votos, MAX(Votos) AS Máximo_Votos
FROM Votos
WHERE ENTIDAD <> 'CIUDAD DE MÉXICO'
GROUP BY ENTIDAD, DISTRITO_FEDERAL
HAVING MIN(Votos) > 150000;

-- Total, promedio, mínimo y máximo de votos por fecha y entidad
-- mostrando solo grupos con votos mayor a 1.500.000 y que los mínimos y máximos sean diferentes

SELECT Fecha_Publicación, ENTIDAD , SUM(Votos) AS Suma_Votos, AVG(Votos) AS Promedio_Votos, MIN(Votos) AS Mínimo_Votos, MAX(Votos) AS Máximo_Votos
FROM Votos
GROUP BY Fecha_Publicación, ENTIDAD
HAVING SUM(Votos) > 1500000 AND MIN(Votos) <> MAX(Votos);

--- Ejercicios de fechas y tiempo

-- Fecha actual con hora, minutos y segundos

SELECT GETDATE () AS Fecha_Y_Hora_Actual;

-- Fecha actual

SELECT CAST(GETDATE() AS DATE) AS Fecha_Actual;

-- Hora actual

SELECT CAST(GETDATE() AS TIME) AS Hora_Actual;

-- Ejemplo dinámico ficticio de suma de ventas del día de hoy

SELECT SUM(Ventas) AS Ventas_Totales_Hoy
FROM Ventas
WHERE Fecha_Venta = CAST(GETDATE() AS DATE);

-- Registros de ventas con la fecha actual

SELECT *
FROM Ventas
WHERE Fecha_Venta = CAST(GETDATE() AS DATE);
