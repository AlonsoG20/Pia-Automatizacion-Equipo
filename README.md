# Planeación de demanda: app Android con Capacitor

Dashboard de planeación de la demanda para una empresa de ejemplo. Muestra indicadores de ventas, pronóstico y precisión, con filtros por región y categoría, gráficas, proyección a 3 meses, gestión de registros (alta, edición y eliminación) e importación y exportación de CSV. Los datos se guardan de forma local en el dispositivo, sin base de datos.

Los datos incluidos son hipotéticos y sirven para demostrar el funcionamiento del sistema.

## Contenido del proyecto

```
planeacion-demanda/
├── www/
│   └── index.html           La app completa (HTML, CSS y JavaScript)
├── capacitor.config.json    Nombre e id de la app
└── README.md
```

## Opción rápida: ver la app sin instalar nada

Abre `www/index.html` con Chrome o Edge. Necesita internet para cargar Chart.js y la tipografía.

## Requisitos para generar el APK

- Node.js (versión LTS), de https://nodejs.org. Para comprobarlo, ejecuta `node -v` en una terminal.
- Android Studio, de https://developer.android.com/studio. Ábrelo una vez y deja que descargue sus componentes y el SDK de Android.
- Conexión a internet.

## Pasos

1. Abre una terminal dentro de la carpeta del proyecto, la que contiene `www` y `capacitor.config.json`. En Windows, escribe `cmd` en la barra de direcciones del Explorador de archivos y presiona Enter.

2. Crea el archivo de dependencias:
   ```
   npm init -y
   ```

3. Instala Capacitor y los plugins (el de compartir permite exportar el CSV desde el celular):
   ```
   npm install @capacitor/core @capacitor/cli @capacitor/android @capacitor/filesystem @capacitor/share
   ```

4. Agrega la plataforma Android:
   ```
   npx cap add android
   ```
   Si pregunta si quieres crear una cuenta de Ionic, responde `n`.

5. Copia la app al proyecto de Android:
   ```
   npx cap sync
   ```

6. Abre el proyecto en Android Studio:
   ```
   npx cap open android
   ```
   Espera a que termine la barra de progreso de Gradle (abajo a la derecha). La primera vez puede tardar varios minutos.

7. Prueba la app. Conecta un celular con la depuración USB activada, o crea un emulador en Device Manager, y presiona el botón verde de Run.

8. Genera el APK desde el menú: `Build > Build Bundle(s) / APK(s) > Build APK(s)`. Al terminar, el archivo queda en:
   ```
   android/app/build/outputs/apk/debug/app-debug.apk
   ```

9. Instálalo copiando el `.apk` al celular. Android pedirá permitir la instalación desde orígenes desconocidos.

## Después de editar la app

Cada vez que cambies `www/index.html`, ejecuta `npx cap sync` y vuelve a compilar el APK.

## Uso sin internet (opcional)

Por defecto la app carga Chart.js desde internet. Para que las gráficas funcionen sin conexión:

1. Descarga https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js y guárdalo dentro de `www`.
2. En `www/index.html`, cambia la línea del `<script src="https://cdnjs...">` por:
   ```
   <script src="chart.umd.min.js"></script>
   ```
3. Ejecuta `npx cap sync`.

## Problemas frecuentes

| Mensaje o síntoma | Solución |
|---|---|
| `Missing appId for new platform` | Confirma que `capacitor.config.json` esté en la misma carpeta que `www`. Si no existe, ejecuta `npx cap init "Planeacion de demanda" mx.fime.planeaciondemanda --web-dir www`. |
| `Could not find the web assets directory` | Debe existir `www/index.html` en la carpeta del proyecto. |
| Gradle tarda mucho o falla | Verifica tu conexión a internet, espera a que termine y usa `File > Sync Project with Gradle Files`. |
| `SDK not found` | En Android Studio abre `Tools > SDK Manager` e instala el SDK de Android. |
| Las gráficas no aparecen | No hay internet al abrir la app. Usa la copia local de Chart.js. |

## Formato del CSV para importar

Columnas obligatorias: `mes`, `region`, `categoria`, `ventas`, `pronostico`. Columna opcional: `estatus` (Completado, Pendiente o En revisión). El mes se escribe como `AAAA-MM`.

```
mes,region,categoria,ventas,pronostico,estatus
2026-10,Norte,Lager,180,175,Completado
```
