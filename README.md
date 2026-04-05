# Proyecto ETL EduData Canarias con Apache Hop

Este proyecto implementa un proceso ETL (Extract, Transform, Load) utilizando Apache Hop, herramienta moderna equivalente a Pentaho Data Integration (PDI).

El objetivo es limpiar y transformar datos educativos procedentes de ficheros CSV con problemas de calidad, como valores vacíos, formatos incorrectos y registros duplicados.

---

# Requisitos

Sistema operativo:

- Linux (Ubuntu/Debian recomendado)

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

## 3. Ejecutar la herramienta (equivalente a Spoon)

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
│   └── salida_limpia.csv
│
├── pipelines/
│   └── limpieza_datos.hpl
│
└── workflows/
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
- formatos inconsistentes
- registros duplicados
- nombres de columnas poco claros

---

# Pipeline ETL

Archivo:

```
pipelines/limpieza_datos.hpl
```

Flujo implementado:

```
CSV file input
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

Carga los datos desde el fichero CSV original.

Función:

- lectura de datos
- detección de columnas

---

## Select values

Permite modificar los campos del dataset.

En este proyecto:

- renombrar columnas
- preparar estructura de datos

Cambios realizados:

| original | nuevo             |
| -------- | ----------------- |
| nombre   | nombre_estudiante |
| curso    | curso_nombre      |

---

## Sort rows

Ordena los datos por el campo id.

Se utiliza para facilitar la detección de duplicados.

---

## Unique rows

Elimina registros duplicados basándose en el campo id.

Ejemplo:
Se elimina la fila repetida con id = 3.

---

## Text file output

Genera el fichero CSV final limpio.

Archivo generado:

```
data/salida_limpia.csv
```

---

# Ejecución del pipeline

Abrir Apache Hop:

```bash
./hop-gui.sh
```

Abrir el pipeline:

```
pipelines/limpieza_datos.hpl
```

Ejecutar:

```
Run ▶
```

El archivo generado se guardará en:

```
data/salida_limpia.csv
```

---

# Resultado final

```csv
nombre_estudiante,curso_nombre,id,edad,email
Ana  ,Python  ,1              ,23 ,ana@email.com
Juan ,Big Data,2              ,,juan@email
Pedro,Hadoop  ,3              ,35 ,pedro@email.com
Maria,SQL     ,4              ,abc,maria@email.com
,Python  ,5              ,29 ,sin_nombre@email.com
```
