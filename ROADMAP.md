# VNG Plataforma Ministerial — Roadmap

> Última actualización: 28/06/26
> Estado general: En uso interno (Iglesia Vida Nueva, Gerli). Pre-comercialización.

---

## ✅ Resuelto / Funcionando

### Módulos
- Rebaños (Miembros) — completo
- Nuevos Contactos — completo
- Grupos de Vida — completo
- Discipulado — base completa
- Orientación Familiar — base completa
- Configuración y Permisos — completo
- Nuevas Generaciones — en DEV, en ajuste

### Infraestructura
- Firebase Auth anónima funcionando en PROD y DEV
- Deploy automático desde GitHub via Vercel
- PROD: `vng-plataforma.vercel.app` → rama `main`
- DEV: `vng-plataforma-git-development-iglesiavngs-projects.vercel.app` → rama `development`
- Login con usuario y contraseña (sistema propio, usuarios guardados en Firebase)

### Fixes aplicados (26/06/26)
- `mergeMembers` — compara `_ts` por miembro, el más reciente gana
- `_ts` agregado a cada miembro al guardar
- `nvSaveInlineForm` — usa `saveK` en lugar de `localStorage` directo
- `nvPromover` — hace `fbUpload` inmediatamente al cambiar condición
- `enviarForm` (QR) — usa `S.members + saveK` en lugar de `localStorage` directo
- `nvCloseInlineForm` — llama a `renderNuevos()` para refrescar lista
- `renderNuevos` — `fTipoGrupo` declarada con fallback seguro
- Campo `fechaIngreso` — cambiado a `type="text"` con formato `dd/mm/aa`, solo numérico
- Permisos: eliminado `allNinguno` check que pisaba configuración del Super Admin

### Fixes aplicados (27/06/26) — Guardado de datos críticos
- `mergeByTs` genérico — merge registro por registro para todos los arrays críticos
- `fbDownload` — protege `gdv_r5`, `gdv_disc_records`, `gdv_ng_records` con merge inteligente
- `_ts` agregado a registros nuevos y editados de GdV, Discipulado y Nuevas Generaciones
- `discGuardarAsistencia` — fbUpload falla ruidosamente si hay error
- Funciones de formato de fecha (`fmtFnac`, `validarFnac`, etc.) expuestas en `window`
- Campo `sa-mf-fnac` y `nv-edit-fnac` — autoformato `dd/mm/aa` con validación
- `markFieldError` — resalta visualmente campos inválidos al guardar
- Dashboard GdV — grupos con encuentros y sin-encuentro en mismo período se muestran correctamente (estado `mixto`)
- Control semanas ISO — no se puede duplicar ni saltear una semana
- Mensajes diferenciados por rol en control de semana duplicada
- `nvCloseInlineForm` — llama `renderNuevos()` al cerrar
- Bloqueo de semanas pendientes al guardar en GdV

### Fixes aplicados (28/06/26) — Permisos y Configuración
- **Fix crítico:** `initStorage` borraba `gdv_permisos` en cada inicio de app — eliminado
- Permisos ahora persisten correctamente entre sesiones y dispositivos
- `cfgSavePermisos` usa `fbSavePermisos` con `gdv_permisos_ts` propio
- `gdv_permisos` excluido del upload/download general — canal propio
- `loadPermisos` — Super Admin tiene control total, sin relleno automático con defaults
- Roles nuevos se inicializan con todo en `ninguno` (no con defaults)
- Fix `[object Object]` en inicialización de permisos de rol nuevo

---

## 🔴 Crítico — Resolver próxima sesión

### `gdvRenderChart is not defined` en Dashboard GdV
- Error al tocar el Dashboard de GdV
- `gdvRenderChart` no está expuesta en `window` o no está definida en el scope correcto
- Mismo patrón que el bug de `fmtFnac` — función no accesible desde HTML

### Control semana ISO para Darío Codina (facilitador)
- Sigue pudiendo cargar dos veces en la misma semana ISO
- El chequeo al abrir el formulario puede no estar funcionando para facilitadores
- Revisar `gdvInitFacForm` y el chequeo automático al seleccionar grupo

---

## 🔴 Crítico — Bloquea comercialización

### Auth real (Firebase Auth email/password)
- Hoy las contraseñas se guardan en texto plano en Firebase
- Sin recupero de contraseña por email
- **Hacer junto con multi-tenancy**

### Multi-tenancy
- Cada iglesia necesita su colección separada en Firebase (`vng_{orgId}/...`)
- Requiere panel de super-admin para gestionar organizaciones
- Onboarding automático para nuevas iglesias

### URL propia
- Definir nombre comercial de la plataforma
- Configurar dominio dedicado (ej: `app.vngplataforma.com`)

---

## 🟡 Importante — Mejora confianza y experiencia

### Indicador visible de sincronización
- Mostrar "✅ Guardado en la nube" o "⚠️ Sin conexión" en tiempo real

### Historial de cambios visible
- Que el usuario vea quién modificó cada registro y cuándo

### Mensaje de estado al abrir
- Mostrar "Datos actualizados al DD/MM/AA HH:MM" al iniciar sesión

### Export / Backup manual
- Botón para descargar todos los datos en Excel o PDF

### Backup automático
- Export semanal automático a Google Drive o similar

### Nuevas Generaciones
- Terminar ajustes en DEV
- Pasar a PROD cuando esté estable

### Edición de registros existentes (Super Admin)
- En GdV, NG y Discipulado

---

## 🟢 Backlog — Ideas y mejoras futuras

- Entorno DEMO con datos ficticios (para ventas y pruebas)
- Notificaciones (email o push) para líderes
- Export PDF de dashboards para informes
- App instalable (PWA) para celular
- Gráficos con Chart.js (evolución visual)
- Modo oscuro
- Tutorial interactivo para nuevos usuarios
- Selector de íconos/fotos para módulos

---

## 📋 Cómo trabajar con este archivo

- **Al abrir un chat nuevo** → compartir este archivo como primer contexto
- **Bugs urgentes** → abrir sesión de corrección, referenciar este archivo
- **Al cerrar cada sesión** → actualizar este archivo con lo resuelto y lo nuevo

---

## 💰 Modelo de negocio (referencia)

| Plan | Precio | Incluye |
|------|--------|---------|
| Básico | USD 20/mes | Hasta 100 miembros, GdV + Rebaños |
| Completo | USD 45/mes | Hasta 300 miembros, todos los módulos |
| Grande | USD 80/mes | Miembros ilimitados + soporte prioritario |

Mercado objetivo: iglesias de 50-500 miembros en Argentina, Uruguay, Chile, Paraguay.

---

## 🔧 Stack técnico

- Un solo archivo HTML con todo el JS/CSS integrado
- Firebase Firestore + Anonymous Auth
- Vercel (deploy automático desde GitHub)
- Repo: `github.com/IglesiaVNG/vng-plataforma`
- Login propio con usuarios guardados en Firebase (migrar a Firebase Auth real)

### Storage keys
```
SK.users='gdv_u5' | SK.groups='gdv_g5' | SK.records='gdv_r5' | SK.members='gdv_m5'
SK.roles='gdv_roles5' | SK.ngComunidades='gdv_ng5' | SK.ngRecords='gdv_ng_records'
SK.modulesConfig='gdv_modules5' | SK.discPasos='gdv_disc_pasos' | SK.discRecords='gdv_disc_records'
```
