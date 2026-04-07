# Proyecto ETL EduData Canarias con Apache Hop

Este proyecto implementa un proceso ETL utilizando Apache Hop, herramienta moderna equivalente a Pentaho Data Integration (PDI).

El objetivo es limpiar y transformar datos educativos procedentes de ficheros CSV con problemas de calidad, como valores vacíos, formatos incorrectos y registros duplicados.

---

# Requisitos

Sistema operativo:

- Linux

Software necesario:

- Java JDK 11 o superior
- Apache Hop

---

# Instalación

## 1. Instalar Java

Apache Hop necesita Java para ejecutarse.

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

Comprobar instalación:

```bash
java -version
```

Debe mostrar algo similar a:

```
openjdk version "17"
```

---

## 2. Descargar Apache Hop

Descargar desde la web oficial:

```bash
wget https://downloads.apache.org/hop/2.9.0/apache-hop-client-2.9.0.zip
```

Descomprimir:

```bash
unzip apache-hop-client-2.9.0.zip
```

Entrar en la carpeta:

```bash
cd hop
```

---

## 3. Ejecutar la herramienta

Dar permisos:

```bash
chmod +x hop-gui.sh
```

Ejecutar:

```bash
./hop-gui.sh
```

Se abrirá la interfaz gráfica de Apache Hop.

---

# Estructura del proyecto

```
ProyectoEduData/
│
├── data/
│   ├── entrada.csv
│   └── limpieza_dato.csv
│
├── pipelines/
│   └── limpieza_dato.hpl
│
└── workflows/
```

Mover la carpeta dentro de hop:

```bash
mv ../ProyectoEduData ./
```

---

# Dataset de entrada

Archivo:

```
data/entrada.csv
```

Contenido:

```csv
id,nombre,edad,email,curso
1,Ana,23,ana@email.com,Python
2,Juan,,juan@email,Big Data
3,Pedro,35,pedro@email.com,Hadoop
3,Pedro,35,pedro@email.com,Hadoop
4,Maria,abc,maria@email.com,SQL
5,,29,sin_nombre@email.com,Python
```

Problemas detectados:

- valores vacíos
- datos incorrectos
- registros duplicados
- formato inconsistente
- nombres de columnas poco claros

---

# Pipeline ETL

Archivo:

```
pipelines/limpieza_dato.hpl
```

Flujo implementado:

```
CSV file input
        ↓
Replace in string
        ↓
String operations
        ↓
If Null
        ↓
Select values
        ↓
Sort rows
        ↓
Unique rows
        ↓
Text file output
```

---

# Explicación de cada componente

## CSV file input

Se cargó el fichero CSV original como fuente de datos.

Función:

- lectura de datos
- detección automática de columnas
- preparación de los datos para su transformación

---

## Replace in string

Se sustituyeron valores incorrectos por valores válidos.

Transformaciones realizadas:

- se reemplazó el valor incorrecto **abc** en el campo edad por **0**
- se corrigió el email incorrecto **juan@email** por **juan@email.com**

Esto permitió estandarizar los datos antes de continuar el proceso ETL.

---

## String operations

Se eliminaron espacios innecesarios al inicio y al final de los campos.

Ejemplo:

```
"Ana   " → "Ana"
"Python  " → "Python"
```

Esto permitió mejorar la calidad de los datos y evitar errores en pasos posteriores.

---

## If Null

Se sustituyeron valores nulos o vacíos por valores por defecto.

Transformaciones aplicadas:

- el campo nombre vacío se reemplazó por **DESCONOCIDO**
- los valores vacíos en edad se sustituyeron por **0**

Esto evita que el dataset final contenga campos vacíos.

---

## Select values

Se renombraron los campos para hacerlos más claros y comprensibles:

| Campo original | Campo nuevo |
|---------------|------------|
| nombre | nombre_estudiante |
| curso | curso_nombre |

También se reorganizó el orden de las columnas:

```
id, nombre_estudiante, edad, curso_nombre, email
```

---

## Sort rows

Se ordenaron los datos por el campo id para facilitar la detección de registros duplicados.

El ordenamiento permite agrupar registros iguales consecutivamente.

---

## Unique rows

Se eliminaron los registros duplicados basándose en el campo id.

Ejemplo:

```
3,Pedro,35,pedro@email.com,Hadoop
3,Pedro,35,pedro@email.com,Hadoop
```

Se conserva únicamente un registro.

---

## Text file output

Se generó un nuevo fichero CSV con los datos limpios y transformados.

Archivo generado:

```
data/limpieza_dato.csv
```

---

# Ejecución del pipeline

Abrir Apache Hop:

```bash
./hop-gui.sh
```

Abrir el pipeline:

```
pipelines/limpieza_dato.hpl
```

Ejecutar:

```
Run ▶
```

El archivo generado se guardará en:

```
data/limpieza_dato.csv
```

---

# Resultado final

```csv
id,nombre_estudiante,edad,curso_nombre,email
1,Ana,23,Python,ana@email.com
2,Juan,0,Big Data,juan@email.com
3,Pedro,35,Hadoop,pedro@email.com
4,Maria,0,SQL,maria@email.com
5,DESCONOCIDO,29,Python,sin_nombre@email.com
```

---

# Relación con Pentaho Data Integration

Apache Hop utiliza el mismo concepto que Pentaho Data Integration:

| Pentaho | Apache Hop |
|--------|------------|
| Spoon | Hop GUI |
| Transformation | Pipeline |
| Job | Workflow |

Apache Hop es una evolución moderna de Pentaho, manteniendo la misma lógica ETL.

---

# Relación con Big Data

Las herramientas ETL son fundamentales en entornos Big Data porque permiten:

- integrar datos de múltiples fuentes
- limpiar datos automáticamente
- mejorar la calidad de la información
- preparar los datos para análisis
- automatizar procesos de transformación

En entornos Big Data, los datos suelen provenir de diferentes sistemas y formatos, por lo que es necesario procesarlos antes de su análisis.

---

# Configuración

Se han utilizado rutas absolutas en:

- archivo de entrada CSV
- archivo de salida CSV

Si el programa genera errores al ejecutarse, se deben modificar las rutas para que coincidan con la ubicación local del proyecto.

Ejemplo:

```
/home/usuario/Apache_hop/hop/ProyectoEduData/data/entrada.csv
```
<img width="1477" height="718" alt="image" src="https://github.com/user-attachments/assets/1d2ac317-125b-4d5b-997d-edae27acd02e" />


<img width="1773" height="805" alt="image" src="https://github.com/user-attachments/assets/3e7e1e1b-5d35-4057-98ca-98db6413b737" />







