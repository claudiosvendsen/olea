# 🫒 Olea Ventas — v2 (100% en el celu)

App que corre **completamente en el iPhone**, sin servidor, sin PC encendida.
Los datos viven en el browser (IndexedDB). La exportación sube un Excel a Google Drive.

---

## Cómo funciona

```
iPhone (Safari)
  └── index.html          ← toda la app
  └── IndexedDB           ← base de datos dentro del browser
       ├── ventas         ← cada venta registrada
       ├── cierres        ← snapshots mensuales cerrados
       └── productos      ← hardcodeados en el JS (del Excel Olea)

    ↓ al exportar

Google Drive
  └── olea_mayo_2025.xlsx
  └── olea_historial.xlsx

    ↓ tu hermano descarga cuando quiera

PC / computadora
```

---

## Instalación — paso a paso

### Paso 1: Configurar Google OAuth (hacerlo una sola vez)

Para que la app pueda subir archivos al Drive de tu hermano necesitás
un **Client ID de Google**. Es gratis y tarda 5 minutos:

1. Ir a https://console.cloud.google.com
2. Crear un proyecto nuevo (o usar uno existente) → dale cualquier nombre
3. En el menú izquierdo: **APIs y servicios → Biblioteca**
4. Buscar "Google Drive API" → habilitarla
5. En el menú: **APIs y servicios → Credenciales**
6. Click en **+ Crear credencial → ID de cliente de OAuth 2.0**
7. Tipo de aplicación: **Aplicación web**
8. Nombre: "Olea app"
9. En **Orígenes autorizados de JavaScript** agregar:
   - `https://TU-USUARIO.github.io` (si lo hospedás en GitHub Pages)
   - O la URL donde lo subas
10. Click **Crear** → te da un **Client ID** (algo como `123456789-abc.apps.googleusercontent.com`)

11. Abrir `index.html` y en la línea que dice:
    ```
    const DRIVE_CLIENT_ID='TU_CLIENT_ID_AQUI.apps.googleusercontent.com';
    ```
    Reemplazar con tu Client ID real.

12. También en **Pantalla de consentimiento OAuth** (menú lateral):
    - Tipo de usuario: **Externo**
    - Agregar el email de tu hermano como "usuario de prueba"

---

### Paso 2: Hospedar el archivo HTML

El HTML necesita estar en una URL `https://` para que:
- Se pueda instalar como PWA en el iPhone
- Google OAuth funcione (no funciona en `file://`)

**Opción más fácil: GitHub Pages (gratis)**

```bash
# 1. Crear cuenta en github.com si no tiene
# 2. Crear repositorio nuevo, llamarlo "olea"
# 3. Subir index.html al repositorio
# 4. En el repo: Settings → Pages → Source: main → /root
# 5. La URL queda: https://TU-USUARIO.github.io/olea
```

**Alternativa: Netlify (también gratis, más fácil)**
- Ir a netlify.com
- Drag & drop de la carpeta con el index.html
- Te da una URL https:// al instante

---

### Paso 3: Instalar en el iPhone

1. Abrir Safari en el iPhone
2. Ir a la URL (`https://tu-usuario.github.io/olea`)
3. Tocar el botón compartir (cuadrado con flecha)
4. **"Agregar a pantalla de inicio"**
5. Queda como ícono en el home, sin Safari visible

---

## Uso diario

### Cargar ventas
1. Entrar a la app
2. Tab **Cargar venta**
3. Elegir fecha (por defecto hoy), producto, tipo (mayor/menor), cantidad
4. **+ Agregar al pedido** → se puede agregar varios productos
5. **Guardar ventas del día**

### Ver el dashboard
- Tab **Hoy** → ventas, ingresos, costos, ganancia del día
- Tab **Mes** → métricas del mes, barras por día, ranking de productos
  - Flechas `‹ ›` para navegar meses anteriores

### Cerrar el mes
- En el tab **Mes**, cuando ya pasó el mes, aparece el botón "Cerrar mes"
- Al cerrarlo se guarda un snapshot permanente
- Después de cerrado, aparece el botón para **exportar a Drive**

### Exportar a Google Drive
1. Tocar **Exportar [Mes] a Drive** (en tab Mes, después de cerrar)
   o **Exportar historial completo** (en tab Historial)
2. Primera vez: pide permiso a Google → logueo con la cuenta de tu hermano
3. Sube el archivo Excel automáticamente
4. Desde la PC, entrar a drive.google.com y descargar

---

## Estructura del Excel exportado

**Por mes (`olea_mayo_2025.xlsx`):**
- Hoja `Resumen` — métricas totales del mes
- Hoja `Detalle ventas` — cada venta registrada
- Hoja `Por producto` — ranking de productos

**Historial (`olea_historial.xlsx`):**
- Hoja `Historial` — todos los meses cerrados
- Hoja `Todas las ventas` — cada venta de toda la historia

---

## Backup de los datos

Los datos están en el **IndexedDB del browser del iPhone**. Para no perderlos:
- Exportar regularmente a Drive ← esto es el backup
- No borrar los datos del sitio en Safari (Configuración → Safari → Avanzado → Datos de sitio web)
- Si se cambia de teléfono, exportar antes de transferir

---

## Precios cargados (del Excel)

| Producto | Costo | Mayor | Menor |
|----------|-------|-------|-------|
| Botella PET 1/2 Lt | $4.370 | $7.500 | $10.000 |
| Botella PET 1 Lt | $7.850 | $14.000 | $18.000 |
| Botella Vidrio 1/2 Lt | $5.805 | $9.800 | $15.000 |
| Vasija Vidrio 1/2 Lt | $6.560 | $11.500 | $18.000 |
| Bidón 5 Litros | $36.450 | $70.000 | $75.000 |
| Frasco Aceituna 1/2 Lt | $3.900 | $6.000 | $8.000 |
| Frasco Aceituna 1 Lt | $7.600 | $11.000 | $14.000 |

Para actualizar precios hay que editar el array `PRODUCTOS` en el `index.html`.

---

## Troubleshooting

**"El botón Drive no hace nada"**
→ Verificar que el Client ID esté puesto correctamente en el HTML
→ Verificar que el email del hermano esté en "usuarios de prueba" en Google Console
→ El sitio debe estar en https:// (no funciona en http:// ni file://)

**"Los datos desaparecieron"**
→ Seguramente se borró el sitio en Safari. Para evitarlo: no tocar "Borrar historial y datos de sitios web" en Configuración de Safari. Exportar a Drive regularmente.

**"Quiero cambiar un precio"**
→ Buscar en index.html la línea con el producto y cambiar `pm:` (precio mayor) o `pme:` (precio menor)
