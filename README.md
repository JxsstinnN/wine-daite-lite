# wine-daite-lite

Runtime de Wine 11.14 para Arch Linux y EndeavourOS, preparado para ejecutar DAITE y otras aplicaciones Visual FoxPro de 32 bits mediante ODBC.

Este repositorio **no contiene DAITE**, prefixes de Wine, configuraciones SQL, contraseñas ni logs.

## Por qué es ligero

El paquete reutiliza el runtime binario oficial de Arch y sustituye únicamente los tres binarios de `odbc32` afectados. Se eliminan headers, manuales, librerías estáticas y herramientas para desarrollar o compilar programas con Wine.

No compila Wine completo y no requiere tener instalado el paquete `wine` normal. `wine-daite-lite` lo reemplaza y lo provee para que Pacman mantenga una sola instalación.

## Arreglos incluidos

- Conserva y aplica las opciones pre-connect `LOGIN_TIMEOUT`, `PACKET_SIZE`, `AUTOCOMMIT` y `ODBC_CURSORS` usadas por Visual FoxPro.
- Corrige los thunks WoW64 de `SQLAllocHandle` y `SQLAllocHandleStd`.
- Añade `TrustServerCertificate=yes` solamente en la ruta Unix de `SQLDriverConnect` y solo cuando la cadena no contiene ya esa propiedad.

El parche fuente completo se conserva en `0001-odbc-wow64-daite.patch`; los tres binarios generados están incluidos para que cada PC no tenga que recompilar Wine.

## Construir el paquete

```bash
sudo pacman -S --needed base-devel
makepkg -s
```

Esto solo descarga y reempaqueta el runtime oficial. No realiza una compilación completa de Wine.

## Instalar

```bash
sudo pacman -U wine-daite-lite-11.14-1-x86_64.pkg.tar.zst
```

Si existe `wine`, Pacman pedirá sustituirlo debido al conflicto declarado. El prefix y los programas Windows no se borran.

## Ejecutar DAITE

```bash
WINEPREFIX="$HOME/.wine-daite-patched" wine-daite "C:\\Sistema\\daite\\DAITE.exe"
```

También puede usarse el comando normal `wine`; ambos apuntan al mismo runtime parcheado.

Antes de cerrar procesos del prefix:

```bash
WINEPREFIX="$HOME/.wine-daite-patched" wineserver -k
```

## Dependencia externa

`unixodbc` se instala como dependencia. Microsoft ODBC Driver 18 for SQL Server debe instalarse separadamente en cada equipo y no se redistribuye en este repositorio.

```bash
odbcinst -q -d
```

## Publicar

Guarda los archivos del repositorio en Git. Para las otras PCs, puedes adjuntar el `.pkg.tar.zst` terminado a una Release e instalarlo directamente con `pacman -U`.
