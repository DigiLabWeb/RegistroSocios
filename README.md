# Registro de Socios — Sindicato de Taxistas Lázaro Cárdenas del Río

Archivos:
- **index.html** + **logo.png** → van a GitHub Pages (los dos en la raíz del repositorio).
- **Code.gs** → va en Apps Script (Extensiones → Apps Script de tu hoja).

## Datos que se registran
Número de socio · Nombre · Fecha de nacimiento · CURP · Concesión · INE (frente y reverso, con clave de elector) · Foto del gafete (opcional).

**Dirección** (obligatoria, se despliega al tocarla): calle, núm. exterior, núm. interior (opcional), colonia, C.P., estado y municipio.
- El C.P. debe tener 5 dígitos y corresponder al estado elegido (al escribir el C.P. el estado se elige solo).
- La dirección no tiene que ser la de la INE. Si sí lo es, el botón **Copiar la dirección de la INE** la llena para revisarla.
- La página no puede comprobar que la colonia exista; solo valida el formato y que el C.P. coincida con el estado.

## Instalar / actualizar Apps Script
1. Pega el `Code.gs` nuevo completo y guarda (💾).
2. **Activa la lectura de INE:** en el menú izquierdo, junto a **Servicios**, toca **+** → busca **Drive API** → **Agregar**.
3. En la lista de funciones elige **`autorizar`** → **▶ Ejecutar** → acepta los permisos.
   Debe decir: *"Listo: permisos concedidos y lectura de INE activada."*
4. **Implementar → Administrar implementaciones → ✏️ → Versión: Nueva versión → Implementar.**
   (Así la URL no cambia.)

La hoja "Socios" reacomoda sus columnas sola; los datos existentes se conservan.

## Cómo funciona el escaneo de INE
1. En "Nuevo socio", toca **Frente** → se abre la cámara con un marco del tamaño de la credencial.
2. Mientras apuntas, el detector avisa en vivo si está **oscura, borrosa, con reflejo o muy lejos**. El marco se pone verde cuando se ve bien.
3. **Ajustar la foto:** después de tomarla (o subirla) se abre un editor para:
   - **Recortar:** arrastrar las esquinas o los lados para dejar solo la credencial; arrastrar el recuadro para moverlo.
   - **Girar** a la izquierda o a la derecha (90°) y **Voltear** (efecto espejo).
   - **Restablecer** el recorte original, **Repetir** la foto o **Aceptar**.
   Mientras se ajusta, el detector indica si el recorte se ve bien.
4. Al aceptar, Google lee el texto y la página revisa que se hayan leído los datos clave (nombre, CURP, fecha, domicilio, clave de elector) y que la fecha coincida con la CURP.
   - **Leída ✓** → llena los campos marcados con la etiqueta *INE*.
   - **Lectura parcial / No se leyó** → explica por qué y pide repetir.
   - Detecta si escaneaste el lado equivocado.
5. Repite con el **Reverso** (se usa la zona de código para confirmar nombre y fecha).
6. **¿Es la misma credencial?** Con los dos lados escaneados (en cualquier orden) se compara el frente con la zona de código del reverso: **nombre, fecha de nacimiento y sexo** (este último sale de la CURP). Si no coinciden, ambos lados se marcan "No coincide" y el socio queda "INE por revisar".
7. **Cuándo se aprueba:** basta con que se pueda leer la credencial — al menos 2 datos clave del frente (nombre, CURP, fecha o clave de elector) y el nombre o la fecha del reverso — y que frente y reverso coincidan.
8. Si no se pudo leer bien, el socio queda como **"INE por revisar"**. En su ficha hay un botón para marcarla como verificada después de revisarla a ojo.

Sin cámara (o si se niega el permiso) se puede **subir una foto** de la INE.

**Orientación:** si la foto viene vertical o de cabeza, la página la endereza sola (la acuesta y, si no se lee, prueba girándola al revés). En la cámara, el botón **↻ Vertical** cambia el marco para escanear la credencial parada.

## Privacidad
- Las imágenes de la INE se guardan en la carpeta **"Registro de Socios - INE (privado)"** de tu Drive, **sin compartir**. La página solo las muestra con sesión iniciada.
- La foto del gafete se guarda pequeña dentro de la hoja (no es pública).
- Al eliminar un socio, sus imágenes se mandan a la papelera de Drive.
- Por tratarse de datos personales, conviene tener un aviso de privacidad para los socios.

## Usuarios
Se definen en `Code.gs` (no en la página):
```js
const USERS = { 'gael': 'admin', 'recepcion': 'otraClave' };
```
Después de cambiarlos: Nueva versión (paso 4).

## Probar sin conexión
Abre `index.html?demo` (gael / admin). Los datos se guardan solo en ese navegador y la lectura de INE usa un ejemplo ficticio.


## Usarla como app (sin tienda de apps)
La página es una **app instalable (PWA)**: se agrega a la pantalla de inicio y se abre a pantalla completa, con su ícono "Socios", sin barra del navegador.
- **Android / Chrome / Edge:** botón **📲 Instalar app** (arriba en la página o debajo de "Entrar"), o menú ⋮ → *Instalar app*.
- **iPhone / iPad (Safari):** botón **📲 Instalar** → muestra los pasos: Compartir ⬆︎ → *Agregar a pantalla de inicio* → Agregar.
- **Computadora:** botón **💻 Instalar app**. En **Chrome** y **Edge** (Windows, Mac, Linux) se instala con un clic y queda con ícono en el escritorio/barra de tareas, en su propia ventana. En **Safari (Mac, macOS 14+)**: Archivo → *Agregar al Dock*. **Firefox** en computadora no permite instalar apps (usar Chrome o Edge).
- Las actualizaciones que subas a GitHub llegan solas a la app la siguiente vez que se abra con internet.
- Los datos de los socios siempre se consultan en línea (no se guardan en el teléfono).

Archivos de la app: `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` (deben estar junto a `index.html`).


## Administración (menú ⚙ Administrar, solo administradores)
**Campos del registro:** agrega campos extra al formulario de socios (Texto, Número, Fecha, Sí/No o Lista de opciones), márcalos como obligatorios, cámbiales el orden (↑ ↓), edítalos o elimínalos. Aparecen en el formulario como "3 · Datos adicionales" y en la ficha del socio.
- Los campos fijos (N.º, nombre, teléfono, fecha de nacimiento, CURP, concesión, dirección, INE, foto) no se pueden quitar.
- Si eliminas un campo, los datos que ya se habían capturado se conservan en la hoja (columna `extras`).

**Usuarios:** agrega, edita (usuario, contraseña, rol) o elimina usuarios.
- **Administrador:** hace todo. **Capturista:** registra y edita socios; no puede eliminarlos ni entrar a Administrar.
- No puedes eliminar tu propio usuario y siempre debe quedar al menos un administrador.
- Contraseñas de al menos 8 caracteres. Se guardan **cifradas** (no se pueden leer, solo reemplazar).

**Mi cuenta:** cualquier usuario toca su nombre (arriba a la derecha) para cambiar su contraseña.

**Nota:** la lista `USERS` de `Code.gs` solo se usa la primera vez para crear los usuarios iniciales. Después, todo se administra desde la app.


## INE: recorte automático y carga rápida
- Al tomar o subir la foto, la app **detecta las 4 esquinas** de la credencial, **quita el fondo** y la **endereza** (aunque esté en ángulo, parada o de lado). Los puntos se pueden mover a mano; abajo se ve "Así se guardará".
- Si quedó de cabeza, el botón **Girar 180°** junto a la vista previa la voltea.
- En la ficha del socio, las dos INE se muestran desde una **copia ligera** (una sola petición). La imagen en alta calidad solo se descarga al tocarla para ampliarla.
- Las copias ligeras se guardan en la pestaña oculta **INE_vista** de la hoja. Las INE registradas antes se aligeran solas la primera vez que se abren.
