# Fixes para ChurrOS

Cosas que encontre revisando el codigo. Los patches estan en la carpeta patches/
y se aplican desde la raiz del repo con: git apply <archivo>.patch

Tabla: tema, que hace el fix, y en que archivo va.

| Tema | Fix | Archivo / patch |
|---|---|---|
| Glassmorphism (transparencia) | Poner window_bg_color y view_bg_color en transparent. libadwaita pintaba el fondo opaco por debajo y tapaba el rgba del CSS | archiso/airootfs/usr/share/churros/styles/churros.css (patch 01-glass-transparencia.patch) |
| Issue 67, pywal cierra todo | El crash no es escribir accent.css, es recargar el CSS en caliente (lo mismo del issue 61). No hay que re-aplicar el provider en runtime al activar pywal | rust/preferences/src/pages/accent.rs (reload_accent_css) y services/pywal.rs (apply_accent) |
| Issue 67, panic extra | adjust() cortaba el hex sin revisar el largo, asi que un color de pywal que no sea rrggbb reventaba la app. Le puse un guard de longitud | rust/preferences/src/services/accent.rs (patch 02-pywal-panic.patch) |
| Glass en XFCE | El picom.conf no tenia blur (cero menciones), por eso XFCE no tenia vidrio esmerilado. Le agregue blur-method dual_kawase | archiso/airootfs/etc/skel/.config/picom.conf (patch 03-xfce-blur-picom.patch) |
| Docs | pywal esta marcado como TODO/pendiente pero ya esta implementado (generate/apply mas el hook en wallpaper.rs) | docs/preferences.md lineas 38, 86, 185 |
| Connectivity | Falta el dialogo de contrasena de WiFi, las redes con clave no se pueden conectar | rust/preferences/src/pages/connectivity.rs:206 (ya hay un TODO) |

Nota: los patches (glass, panic, blur) estan escritos pero sin probar en la ISO
porque compilar necesita Arch. Hay que aplicarlos, compilar y ver. El del blur de
XFCE es el mas seguro (literal faltaba). Los de niri/GTK son el candidato mas probable.

Revisado y salio limpio (no eran bugs): el CI pasa (bash -n, paquetes duplicados),
las traducciones po estan completas, todos los Exec de los .desktop resuelven, y
picom, bazaar y xfce4-terminal si estan en packages.xfce.
