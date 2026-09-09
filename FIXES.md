# Fixes para ChurrOS

Cosas que encontre revisando el codigo. Los patches estan en patches/ y se
aplican desde la raiz del repo de ChurrOS con: git apply <archivo>.patch

Estado de cada uno esta en la tabla (probado / candidato sin probar).

| Tema | Estado | Fix | Archivo / patch |
|---|---|---|---|
| Glass en XFCE | PROBADO, funciona | El picom.conf no tenia blur. Agregue blur-method dual_kawase | archiso/airootfs/etc/skel/.config/picom.conf (patch 03) |
| Glass en niri | Candidato sin probar | Las apps que si se ven con glass (foot, fuzzel, mako) tienen linea opacity ademas del blur; las apps churros solo tenian blur. Agrego opacity 0.9 a sus 4 window-rules | archiso/airootfs/etc/skel/.config/niri/config.kdl (patch 04) |
| Issue 67, panic pywal | Candidato sin probar | adjust() cortaba el hex sin revisar largo; un color pywal no-rrggbb reventaba la app. Guard de longitud | rust/preferences/src/services/accent.rs (patch 02) |
| Issue 67, crashea todo | Diagnostico | El crash es recargar CSS en caliente (mismo del issue 61), no escribir accent.css. No re-aplicar provider en runtime | rust/preferences/src/pages/accent.rs y services/pywal.rs |
| Docs pywal | Nota | pywal marcado como TODO pero ya esta implementado | docs/preferences.md lineas 38, 86, 185 |
| WiFi con clave | Pendiente | Falta el dialogo de contrasena de WiFi | rust/preferences/src/pages/connectivity.rs:206 |

Notas honestas:
- niri confirmado 26.04-1 (soporta blur), la version NO era el problema.
- El fix de niri (opacity) copia el patron de foot/fuzzel/mako que si funciona en
  ese mismo niri. Si el texto queda muy transparente, subir a 0.95. Si asi tampoco
  aparece el glass, no es esto: revisar si foot se ve con blur en niri y correr
  niri validate.
- Los candidatos no los pude probar (compilar ChurrOS necesita Arch).
