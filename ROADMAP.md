# VNG Plataforma Ministerial — Roadmap

> Última actualización: 26/06/25
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

### Fixes aplicados (26/06/25)
- `mergeMembers` — ahora compara `_ts` por miembro, el más reciente gana (ya no se pisan condiciones entre dispositivos)
- `_ts` agregado a cada miembro al guardar
- `nvSaveInlineForm` — usa `saveK` en lugar de `localStorage` directo
- `nvPromover` — ahora hace `fbUpload` inmediatamente al cambiar condición
- `enviarForm` (QR) — usa `S.members + saveK` en lugar de `localStorage` directo
- `nvCloseInlineForm` — ahora llama a `renderNuevos()` para refrescar lista
- `renderNuevos` — `fTipoGrupo` declarada con fallback seguro (evitaba crash al dar de alta)
- Campo `fechaIngreso` — cambiado a `type="text"` con formato `dd/mm/aa`, solo numérico

---

## 🔴 Crítico — Bloquea comercialización

### Auth real (Firebase Auth email/password)
- Hoy las contraseñas se guardan en texto plano en Firebase
- Cualquiera con acceso directo a Firebase ve las credenciales
- Sin recupero de contraseña por email
- **Hacer junto con multi-tenancy**

### Multi-tenancy
- Hoy todos los datos de todas las iglesias estarían mezclados
- Cada iglesia necesita su colección separada en Firebase (`vng_{orgId}/...`)
- Requiere panel de super-admin para gestionar organizaciones
- Onboarding automático para nuevas iglesias
- **Es el paso más importante antes de vender a otras iglesias**

### URL propia
- Definir nombre comercial de la plataforma
- Configurar dominio dedicado (ej: `app.vngplataforma.com`)

---

## 🟡 Importante — Mejora confianza y experiencia

### Indicador visible de sincronización
- Mostrar "✅ Guardado en la nube" o "⚠️ Sin conexión" en tiempo real
- Hoy el usuario no sabe si sus datos subieron correctamente

### Historial de cambios visible
- Que el usuario vea quién modificó cada registro y cuándo
- Genera confianza inmediata en equipos de trabajo

### Mensaje de estado al abrir
- Mostrar "Datos actualizados al DD/MM/AA HH:MM" al iniciar sesión

### Export / Backup manual
- Botón para descargar todos los datos en Excel o PDF
- Si el usuario siente que puede llevarse sus datos, confía más

### Backup automático
- Al menos un export semanal automático a Google Drive o similar
- Hoy si alguien borra datos por error, no hay forma de recuperarlos

### Módulo de Comunicaciones / Eventos
- Pendiente de desarrollo

### Nuevas Generaciones
- Terminar ajustes en DEV
- Pasar a PROD cuando esté estable

### Edición de registros existentes (Super Admin)
- En GdV, NG y Discipulado
- Pendiente

---

## 🟢 Backlog — Ideas y mejoras futuras

- Notificaciones (email o push) para líderes
- Export PDF de dashboards para informes
- App instalable (PWA) para celular
- Gráficos con Chart.js (evolución visual)
- Modo oscuro
- Tutorial interactivo para nuevos usuarios
- Selector de íconos/fotos para módulos

---

## 📋 Cómo trabajar con este archivo

- **Bugs urgentes** → abrir sesión de corrección, referenciar este archivo
- **Features nuevas** → mover de Backlog a Importante/Crítico cuando corresponda
- **Al cerrar cada sesión** → actualizar este archivo con lo resuelto y lo nuevo que aparezca
- **Al abrir un chat nuevo** → compartir este archivo como primer contexto

---

## 💰 Modelo de negocio (referencia)

| Plan | Precio | Incluye |
|------|--------|---------|
| Básico | USD 20/mes | Hasta 100 miembros, GdV + Rebaños |
| Completo | USD 45/mes | Hasta 300 miembros, todos los módulos |
| Grande | USD 80/mes | Miembros ilimitados + soporte prioritario |

Mercado objetivo inicial: iglesias de 50-500 miembros en Argentina, Uruguay, Chile, Paraguay.

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
