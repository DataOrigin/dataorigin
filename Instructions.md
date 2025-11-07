# COMO USAR EL PAQUETE


1. Crea un proyecto desde Google Cloud y habilita Google Sheets API y Google Drive API

2. Ve a Credenciales y crea una Cuenta de Servicio. Descarga la clave como un archivo JSON y guárdalo en una carpeta que sea de fácil acceso.

3. Crea un proyecto de Python y crea o añade un entorno virtual

4. En la consola escribe pip install -r requirements.txt 

5. Comparte la carpeta o los spreadsheets con el correo de la Service Account (campo client_email del JSON) con permiso Editor

6. Si usas Service Account por archivo, crea la variable de entorno GOOGLE_APPLICATION_CREDENTIALS con la ruta absoluta al JSON

7. (Opcional) Crea GDRIVE_FOLDER_ID con el ID de la carpeta de Drive donde quieres crear los spreadsheets (de la URL .../folders/{ID})

8. (Opcional) Crea SPREADSHEET_ID con el ID del spreadsheet a leer por defecto (de la URL .../spreadsheets/d/{ID}/...)

9. Cierra y vuelve a abrir la terminal o la app tras crear variables persistentes

### Cómo guardar la ruta a Service_Account.JSON en el sistema:

- **Windows (CMD):**
  ```cmd
  setx GOOGLE_APPLICATION_CREDENTIALS "C:\ruta\a\tu\Service_Account.JSON"
  ```
  (Cierra y vuelve a abrir CMD para que se aplique)

- **Windows (PowerShell):**
  ```powershell
  [System.Environment]::SetEnvironmentVariable('GOOGLE_APPLICATION_CREDENTIALS','C:\ruta\a\tu\Service_Account.JSON','User')
  ```
  (Cierra y vuelve a abrir PowerShell para que se aplique)

- **Linux/macOS (bash/zsh - persistente, edita ~/.bashrc o ~/.zshrc):**
  ```bash
  echo 'export GOOGLE_APPLICATION_CREDENTIALS="/ruta/a/tu/Service_Account.JSON"' >> ~/.bashrc
  # O para zsh
  # echo 'export GOOGLE_APPLICATION_CREDENTIALS="/ruta/a/tu/Service_Account.JSON"' >> ~/.zshrc
  source ~/.bashrc # O source ~/.zshrc para aplicar inmediatamente
  ```
  (Abre una nueva terminal o ejecuta `source` para aplicar)

10. Verifica las variables: en PowerShell `echo $env:NOMBRE`, en CMD `echo %NOMBRE%`

11. Prueba una escritura/lectura con utils/google_sheets.py para confirmar acceso


# Service Account por archivo
[System.Environment]::SetEnvironmentVariable('GOOGLE_APPLICATION_CREDENTIALS','C:\DataOrigin\secrets\service_account.json','User')

# Service Account como variable (JSON completo)
[System.Environment]::SetEnvironmentVariable('GOOGLE_SERVICE_ACCOUNT_JSON',(Get-Content -Raw -Encoding UTF8 'C:\DataOrigin\secrets\service_account.json'),'User')



# Opcionales
[System.Environment]::SetEnvironmentVariable('GDRIVE_FOLDER_ID','TU_FOLDER_ID','User')
[System.Environment]::SetEnvironmentVariable('SPREADSHEET_ID','TU_SPREADSHEET_ID','User')


-------------------------------------------------------------------------------------------------------------



# COMO ACTUALIZAR EL PAQUETE

Pasos para Actualizar tu Proyecto en PyPI:

Este documento detalla el proceso para actualizar tu paquete Python en PyPI. Es crucial seguir estos pasos en orden para asegurar una actualización exitosa.

### 1. Preparar y Revisar Archivos del Proyecto

Asegúrate de que los siguientes archivos en la raíz de tu proyecto (`C:\DataOrigin\utils`) estén actualizados y correctos:

*   **`pyproject.toml`**:
    *   **Incrementa el número de versión**: Esto es OBLIGATORIO para cada nueva publicación (ej., de "0.1.3" a "0.1.4").
    *   **`description`**: Asegúrate de que la descripción sea precisa.
    *   **`project.urls`**: Verifica que las URLs (`Homepage`, `Bug Tracker`, `Website`) sean correctas y apunten a los lugares adecuados.
    *   **`dependencies`**: Asegúrate de que todas las librerías que tu proyecto necesita estén listadas.
    *   **`license`**: Confirma que la expresión SPDX (`license = {text = "GPL-3.0-or-later"}`) sea la correcta para tu licencia.

*   **`README.md`**:
    *   Actualiza la descripción del proyecto, instrucciones de instalación y uso si es necesario.

*   **`LICENSE`**:
    *   Asegúrate de que el contenido de tu licencia (GPLv3 en este caso) sea el que deseas.

### 2. Instalar Herramientas de Construcción y Publicación

Si aún no las tienes instaladas, ejecuta este comando en tu terminal (desde el directorio del proyecto):

```bash
pip install build twine
```

### 3. Limpiar Artefactos de Construcción Antiguos

Antes de construir una nueva versión, es crucial eliminar los directorios de compilación anteriores para evitar que se suban metadatos obsoletos.

*   **Para Windows (PowerShell):**
    ```powershell
    Remove-Item -Path "dist" -Recurse -Force
    Remove-Item -Path "dataorigin.egg-info" -Recurse -Force
    ```

*   **Para Linux/macOS:**
    ```bash
    rm -rf dist
    rm -rf dataorigin.egg-info
    ```

### 4. Construir el Paquete de Distribución

Una vez que los directorios de construcción antiguos han sido eliminados y `pyproject.toml` está actualizado, genera los nuevos archivos de distribución:

```bash
python -m build
```

Este comando creará los archivos `.whl` y `.tar.gz` en el directorio `dist/`.

### 5. Subir el Paquete Actualizado a PyPI

Finalmente, sube tu paquete a PyPI. Necesitarás un token de API de PyPI (generado en `pypi.org/manage/account/token/`).

Ejecuta el siguiente comando en tu terminal (desde el directorio del proyecto):

```bash
twine upload dist/*
```

Cuando se te solicite:
*   **Token:** Pega tu token de API de PyPI (comienza con `pypi-`).

Si la subida es exitosa, tu paquete actualizado estará disponible en PyPI.

---
