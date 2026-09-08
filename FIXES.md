# Fixes para ChurrOS

Encontrados revisando el código. Los `.patch` están en `patches/`
(se aplican desde la raíz del repo con `git apply <archivo>.patch`).

| # / tema | Fix | Dónde |
|---|---|---|
| **Glassmorphism** (transparencia) | `@define-color window_bg_color transparent;` (+ `view_bg_color`) — libadwaita pintaba opaco por debajo | `archiso/airootfs/usr/share/churros/styles/churros.css` · patch `01-glass-transparencia.patch` |
| **#67** pywal cierra todo | El crash no es escribir `accent.css`, es **recargar CSS en caliente** (mismo demonio del #61). No re-aplicar provider en runtime al activar pywal | `rust/preferences/src/pages/accent.rs` (`reload_accent_css`) y `services/pywal.rs` (`apply_accent`) |
| **#67** panic extra | `adjust()` cortaba el hex sin validar largo → un color de pywal no-`#rrggbb` reventaba la app. Añadido guard de longitud | `rust/preferences/src/services/accent.rs` · patch `02-pywal-panic.patch` |
| **Docs** | pywal marcado como "TODO/pendiente" pero ya está implementado (generate/apply + hook en `wallpaper.rs`) | `docs/preferences.md` líneas 38, 86, 185 |
| **Connectivity** | Falta el diálogo de contraseña WiFi (redes con clave no se pueden conectar) | `rust/preferences/src/pages/connectivity.rs:206` (TODO existente) |

> Nota: los patches de glass y del panic están escritos pero **sin probar en la ISO**
> (se necesita Arch para compilar). Probar y ajustar si hace falta.
