# VNG Plataforma Ministerial — Roadmap

> Última actualización: 09/07/26
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
- `mergeByTs` genérico — merge registro por registro para todos los arrays críticos
- `fbDownload` — protege `gdv_r5`, `gdv_disc_records`, `gdv_ng_records` con merge inteligente
- `_ts` agregado a registros nuevos y editados de GdV, Discipulado y Nuevas Generaciones
- Funciones de formato de fecha expuestas en `window` — corrige "is not defined"
- Campos fecha unificados a `dd/mm/aa` en todos los formularios
- Control semanas ISO — no se puede duplicar ni saltear
- Mensajes diferenciados por rol en control de semana duplicada
- Dashboard GdV — grupos mixtos (con y sin encuentro) se muestran correctamente
- Permisos: `initStorage` ya no borra `gdv_permisos` al iniciar
- `cfgSavePermisos` usa canal propio `fbSavePermisos` con timestamp correcto
- Permisos: Super Admin tiene control total sin relleno automático de defaults

### Fixes aplicados (09/07/26)
- **Campo DNI** — agregado en formulario Nueva Persona (Rebaños) y Alta/Editar en Nuevos Contactos
  - Opcional, máx 9 dígitos, solo numérico
  - No afecta datos existentes
- **Selector Grupo/Comunidad** en Nuevos Contactos — permite asignar GdV o comunidad NG al dar de alta o editar
- **Botón renombrado** — "Nuevo miembro" → "Nueva persona" en Rebaños
- **DNI y Fecha Ingreso** en la misma fila en formulario Rebaños (solo visible para Super Admin)
- **Tab "Roles" → "Funciones"** en Configuración
- **Imagen de fondo** en pantalla de login y toda la app
- **Logo** con fondo transparente — pendiente reemplazo (logo nuevo llega mañana)
- **Sincronización Funciones → Rebaños** — al asignar función en Configuración Usuarios o en comunidad NG, se refleja automáticamente en Rebaños
  - Líder NG y Colaborador se distinguen correctamente
  - Usa comparación por String para evitar problemas de precisión con IDs grandes
- **`userRoles` y `hasRole`** — protegidos contra `currentUser.name/role` null
- **Duplicados en S.members** eliminados (Leandro Perez y Maia Iañez)

---

## 🔴 Crítico — Resolver próxima sesión

### `gdvRenderChart is not defined` en Dashboard GdV
- Error al tocar la pestaña de gráficos del Dashboard GdV
- Función no expuesta en `window`

### Logo con fondo transparente
- Logo actual tiene fondo negro que se mezcla con la imagen de fondo
- Pendiente: reemplazar con PNG transparente que llega mañana
- Una vez reemplazado → subir todo a PROD

### Sincronización Configuración Usuarios → Rebaños
- Al guardar usuario en Configuración, el rol se sincroniza con `m.roles`
- Verificar que funciona correctamente para todos los casos (edición y alta)

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

### IDs de personas
- Los IDs nuevos usan `Date.now()` (timestamp) → números muy grandes
- A futuro: implementar secuencia incremental simple (1, 2, 3...)
- No es urgente pero mejora legibilidad y debugging

### Indicador visible de sincronización
- Mostrar "✅ Guardado en la nube" o "⚠️ Sin conexión" en tiempo real

### Historial de cambios visible
- Que el usuario vea quién modificó cada registro y cuándo

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
