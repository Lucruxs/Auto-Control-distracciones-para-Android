# Control Parental Local para Firefox (Desktop + Android)

Extensión Manifest V2 compatible con Firefox desktop, Firefox Nightly/Beta para Android y forks basados en Gecko (IceRaven, Mull). Mantiene la misma lógica que la versión de Edge: listas de URLs, modo "solo permitidos", protección por PIN y exportación/importación local.

## Funcionalidades
- Listas de URLs permitidas/bloqueadas con patrones (`*.sitio.com`).
- Modo "solo permitidos" con indicador en el icono (badge "ON").
- Panel protegido por PIN (hash + salt) y pista opcional.
- Exportación/Importación local para sincronizar entre escritorio y móvil.
- Página de bloqueo amigable con explicación del motivo.

## Instalación en Firefox Desktop
### Carga temporal (debug)
1. Abre Firefox desktop y visita `about:debugging#/runtime/this-firefox`.
2. Haz clic en **Cargar complemento temporal** y selecciona `manifest.json` dentro de `src/firefox-extension`.
3. El complemento permanecerá activo hasta cerrar Firefox.

### Instalación permanente
1. Empaqueta la carpeta `src/firefox-extension` en un `.xpi` (usa `Compress-Archive` en Windows o `zip -r` en macOS/Linux).
2. **Nightly/Beta:** instala el `.xpi` desde `about:addons` → "Instalar complemento desde archivo".
3. **Firefox estable:** Mozilla requiere firma. Súbelo a [addons.mozilla.org](https://addons.mozilla.org/developers/) como complemento no listado y usa el `.xpi` firmado que descargues.

## Instalación en Firefox Android / IceRaven / Mull
1. Empaqueta `src/firefox-extension` en `control-parental-firefox.xpi` siguiendo [docs/install-firefox-android.md](docs/install-firefox-android.md).
2. Copia el `.xpi` al dispositivo.
3. Desde el navegador (Nightly, IceRaven o Mull) abre el archivo o navega a `file:///sdcard/Download/control-parental-firefox.xpi` para instalarlo.
4. Ve a `about:addons` y habilítalo también en navegación privada si lo necesitas.

## Exportar/Importar
- El formato JSON es idéntico al de Edge, por lo que puedes exportar en un navegador e importar en otro sin perder listas ni PIN.
- Si el archivo es más antiguo que la configuración local, la importación solicitará confirmación antes de sobrescribir.

## Estructura relevante
```
src/firefox-extension/
  manifest.json (MV2)
  background.js
  popup.html / popup.js
  options.html / options.js
  blocked.html / blocked.js / blocked.css
  styles.css
```

## Desarrollo
- `background.js` contiene la lógica de filtrado, hashing de PIN, almacenamiento (`browser.storage.local`) y exportación/importación.
- Las UIs son iguales a las de Edge, usando HTML/CSS/JS vanilla.
- Pruebas recomendadas: bloquear dominios, modo "solo permitidos", exportar/importar entre escritorio y móvil, navegación privada.
- Para distribuir públicamente, publica el `.xpi` en AMO marcando la compatibilidad Android.
