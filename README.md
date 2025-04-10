# Creación de un Contenedor Docker con Miniconda y Jupyter Notebook

Este documento explica cómo crear un contenedor Docker con Miniconda, instalar Jupyter Notebook y configurarlo para tener permisos de **root** dentro del contenedor. Esto te permitirá trabajar con Python y Jupyter de forma aislada sin afectar tu sistema local.

## 1. Crear el Dockerfile

Para crear un contenedor Docker con Miniconda y Jupyter Notebook, primero necesitas crear un archivo `Dockerfile` que define la imagen del contenedor.

Crea un archivo `Dockerfile` en tu directorio de trabajo:

```bash
touch Dockerfile
```

Abre el archivo `Dockerfile` y añade lo siguiente:

```Dockerfile
# Usar una imagen base de Miniconda
FROM continuumio/miniconda3:latest

# Establecer el directorio de trabajo dentro del contenedor
WORKDIR /workspace

# Instalar Jupyter Notebook
RUN conda install -y notebook && conda clean -afy
```

**Explicación**:
- `FROM continuumio/miniconda3:latest`: Utiliza la imagen oficial de **Miniconda**.
- `WORKDIR /workspace`: Establece el directorio de trabajo dentro del contenedor en `/workspace`.
- `RUN conda install -y notebook && conda clean -afy`: Instala **Jupyter Notebook** y limpia la caché de **conda** para reducir el tamaño de la imagen.

## 2. Construir la Imagen Docker

Ahora que tienes el `Dockerfile`, construye la imagen Docker con el siguiente comando:

```bash
docker build -t miniconda-jupyter .
```

**Explicación**:
- `docker build -t miniconda-jupyter .`: Este comando construye la imagen y la etiqueta como `miniconda-jupyter`.

## 3. Ejecutar el Contenedor

Después de construir la imagen, puedes ejecutar el contenedor con el siguiente comando:

```bash
docker run -it --rm -p 8888:8888 -v ~/mis_proyectos:/workspace --user root miniconda-jupyter
```

**Explicación de las opciones**:
- `-it`: Ejecuta el contenedor en modo interactivo, permitiendo interacción en la terminal.
- `--rm`: Elimina el contenedor automáticamente cuando se detenga.
- `-p 8888:8888`: Expondrás el puerto 8888 del contenedor para acceder a Jupyter Notebook en tu navegador.
- `-v ~/mis_proyectos:/workspace`: Monta el directorio `~/mis_proyectos` desde tu máquina local al contenedor en `/workspace`.
- `--user root`: Ejecuta el contenedor como el usuario **root**, dándote permisos completos dentro del contenedor.

## 4. Acceder a Jupyter Notebook

Después de ejecutar el contenedor, en la terminal aparecerá una URL como esta:

```
http://127.0.0.1:8888/tree?token=<TOKEN>
```

Abre esa URL en tu navegador para acceder a Jupyter Notebook, donde podrás crear y editar notebooks de Python.

## 5. Verificar los Permisos de Root

Para verificar que tienes permisos de **root** dentro del contenedor, ejecuta:

```bash
whoami
```

Este comando debería devolver `root`, confirmando que tienes permisos completos dentro del contenedor.

## 6. Beneficios de Usar Root en el Contenedor

Al ejecutar el contenedor como **root**, tienes los siguientes beneficios:
- **Acceso completo** para gestionar archivos dentro del contenedor.
- **Instalación de paquetes y configuraciones** sin restricciones.

## 7. Ejemplos de Uso

### 7.1 Crear un Entorno Virtual con Conda
Dentro del contenedor, puedes crear un entorno virtual para trabajar en proyectos específicos. Por ejemplo:

```bash
conda create -n mi_entorno python=3.9 -y
conda activate mi_entorno
```

Esto crea un entorno llamado `mi_entorno` con Python 3.9 y lo activa.

### 7.2 Instalar Librerías Adicionales
Puedes instalar librerías adicionales dentro del contenedor. Por ejemplo, para instalar `numpy` y `pandas`:

```bash
conda install numpy pandas -y
```

### 7.3 Crear un Notebook de Ejemplo
1. Accede a Jupyter Notebook desde la URL proporcionada.
2. Crea un nuevo notebook de Python.
3. Escribe el siguiente código para probar que todo funciona correctamente:

```python
import numpy as np
import pandas as pd

# Crear un DataFrame de ejemplo
data = {'Columna1': [1, 2, 3], 'Columna2': [4, 5, 6]}
df = pd.DataFrame(data)

print("DataFrame de ejemplo:")
print(df)
```

Ejecuta la celda y verifica que el DataFrame se imprime correctamente.

### 7.4 Guardar tu Trabajo
Todos los archivos creados en el directorio `/workspace` dentro del contenedor se sincronizan con el directorio `~/mis_proyectos` en tu máquina local. Asegúrate de guardar tus notebooks en esa ubicación para no perderlos.

## 8. Limpiar el Contenedor

Si deseas detener el contenedor, puedes usar `Ctrl+C`. Para eliminar contenedores detenidos, ejecuta:

```bash
docker container prune
```

Este comando eliminará todos los contenedores detenidos y liberará espacio en tu sistema.

## 9. Eliminar la Imagen y Contenedores

Si ya no necesitas la imagen ni los contenedores, puedes eliminarlos con los siguientes comandos:

1. Eliminar la imagen Docker:

   ```bash
   docker rmi miniconda-jupyter
   ```

2. Eliminar todos los contenedores detenidos:

   ```bash
   docker container prune
   ```

3. Eliminar todos los volúmenes no utilizados:

   ```bash
   docker volume prune
   ```

## 10. Conclusión

Con estos pasos, has creado un entorno aislado en Docker con **Miniconda** y **Jupyter Notebook**. Este entorno te permite trabajar con Python y Jupyter sin afectar tu sistema local. Además, al ejecutar el contenedor como **root**, tienes acceso completo para instalar y modificar lo que necesites dentro del contenedor.

**Docker** te proporciona un entorno reproducible y aislado, lo cual es ideal para mantener consistencia entre diferentes entornos de desarrollo.

---
