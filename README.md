# Motor de Pricing MyCOCOS Chile 🥥

App web de pricing dinámico con conexión a Google Sheets, tasa de cambio en vivo y multiplicadores estacionales.

---

## 📁 Archivos del repositorio

| Archivo | Descripción |
|---|---|
| `index.html` | Pantalla de login con protección por contraseña (SHA-256) |
| `pricing.html` | Motor de pricing completo — todas las fórmulas están aquí |
| `README.md` | Este archivo |

---

## 🚀 Cómo publicar en GitHub Pages (paso a paso)

### 1. Crear el repositorio en GitHub
1. Ir a [github.com/new](https://github.com/new)
2. Nombre del repo: `mycocos-pricing` (o el que quieras)
3. Visibility: **Private** (recomendado — igual la app tiene contraseña)
4. Clic en **Create repository**

### 2. Subir los archivos
1. En la página del repo, clic en **"uploading an existing file"**
2. Arrastrá los 3 archivos: `index.html`, `pricing.html`, `README.md`
3. Clic en **Commit changes**

### 3. Activar GitHub Pages
1. Ir a **Settings** → **Pages** (menú lateral)
2. En "Source": seleccioná **Deploy from a branch**
3. Branch: **main** · Folder: **/ (root)**
4. Clic en **Save**
5. En ~2 minutos tu URL será: `https://TU-USUARIO.github.io/mycocos-pricing/`

### 4. Compartir con el equipo
- Mandá la URL + la contraseña por separado
- La contraseña por defecto es: **mycocos2025**
- ⚠️ **Cambiala** antes de compartir (ver sección abajo)

---

## 🔐 Cambiar la contraseña

1. Ir a [sha256.online](https://emn178.github.io/online-tools/sha256.html)
2. Escribí tu nueva contraseña → copiá el hash resultante
3. Abrí `index.html` con cualquier editor de texto
4. Buscá la línea: `const PASS_HASH = "..."`
5. Reemplazá el hash con el nuevo
6. Volvé a subir `index.html` al repo de GitHub

---

## 🔗 Conexión con Google Sheets

Los 4 sheets ya están pre-configurados en `pricing.html`. Para que funcionen:

1. Abrí cada Google Sheet
2. **Archivo → Compartir → Cualquiera con el link puede VER**
3. No es necesario que sea público — solo que tenga acceso con link

### IDs pre-cargados:
- **Ventas:** `1ZmdN7AL4Qtfm9cmen632Z-UGoPJuQ1GpzxzMKMcmKTc`
- **Stock:** `1YNcDyuHORqRgv8XFXr0-jZplFWnVximomIQupDeTdxs`
- **Maestro:** `17J4y-dPNuX2vswePSVTfaXyDtB1ekgOYGsoCkced_8M`
- **Velocidad/Recetas:** `1jSV5npc1e9f6Xps-1yyE03JXRNfd8CsGoF1unlORI8w`

---

## ⚙️ Fórmulas de cálculo (todas en pricing.html)

```
1. Costo efectivo = costoBruto (CLP maestro)
   → Si es pack sin costo: suma de componentes × qty del recetario

2. Precio base = costoEfectivo / (1 - margenBase)
   → margenBase default: 65%

3. Precio ajustado = precioBase × ajusteStock × multiplicadorSemana
   → ajusteStock: +8% si cobertura < 20 días / -8% si > 90 días
   → multiplicadorSemana: viene del sheet velocidad (w1–w52 por SKU)

4. Precio final = redondear(precioAjustado × boostTemporada, 100)
   → boostTemporada: 0–7% según temporada seleccionada manualmente

5. Margen real = (precioFinal - costoEfectivo) / precioFinal × 100
```

---

## 🗓️ Temporadas Chile configuradas

| Temporada | Boost manual |
|---|---|
| CyberDay (mayo) | +5% |
| Cyber Lunes | +4% |
| Día del Padre | +6% |
| Navidad | +7% |
| 18 de Septiembre | +5% |
| Hot Sale | +3% |
| San Valentín | +4% |

> Nota: el multiplicador semanal del sheet de velocidad se aplica automáticamente. Los boosts de temporada son adicionales y manuales.

---

## 🔄 Actualizar datos

Los datos se cargan en tiempo real desde Google Sheets cada vez que se hace clic en "↻ Actualizar". No hay caché — siempre muestra los datos más frescos.

Para actualizar el código de fórmulas: editá `pricing.html` directamente y volvé a subirlo al repo.
