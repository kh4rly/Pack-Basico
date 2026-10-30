# Pack-Basico
Atmosphere y Hekate funcional Atualizado
#
CTCaer (Hekate): https://github.com/CTCaer

SciresM (Atmosphere): https://github.com/SciresM

rashevskyv (DBI): https://github.com/rashevskyv/dbi



Compatibilidad HOS 23.0.1


# SP Updater

Aplicacion homebrew para Nintendo Switch con interfaz blanca estilo Horizon.
Descarga y actualiza Atmosphere, firmware y packs, guarda tus partidas en la nube
de Telegram y mantiene tu consola limpia, todo sincronizado con un bot de Telegram.

- **Version:** 1.6.x (autoincremental en cada compilacion)
- **Autor:** N3DSwitch
- **Plataforma:** Nintendo Switch (Atmosphere + hbmenu)
- **Servidor:** Python (FastAPI + python-telegram-bot), pensado para una Raspberry Pi

---

## Funciones de la aplicacion

### Estado
- Version de firmware (HOS) y de Atmosphere, con indicador de emuMMC (`E` / `S`).
- Temperaturas SoC y PCB.
- Velocidades reales de CPU, GPU y RAM.
- Bateria, cargador y direccion IP.

### Tarjeta SD
- Verificacion de la SD (pruebas de escritura/lectura con varios tamanos).
- Velocidad de lectura y escritura medida.
- CID, fabricante (MID), marca y estimacion de autenticidad de la tarjeta.
- Modo de velocidad de la SD.
- Fix Archive Bit: escanea y corrige lo que puede; si es necesario, recomienda
  hacerlo desde Hekate y permite reiniciar a Hekate.

### Sesion
- Registro/inicio de sesion con el bot: usuario + ID de descarga + serial.
- Guarda la sesion en la SD y recuerda los datos para la proxima vez.
- Comprobacion de conexion con el bot.
- **Nick provisional** para usuarios sin cuenta: se usa para identificar sus
  guardados y migrarlos cuando inicien sesion.
- **Cerrar sesion** desvincula la consola del bot (para poder vincular otra).
- Selector de idioma (sistema, espanol, ingles, frances, aleman, italiano,
  portugues y japones).

### Descargas
- **Pack Basico (Atmosphere)** 鈥?sin iniciar sesion.
- **Firmware (NX_Firmware)** 鈥?sin iniciar sesion.
- **Pack VIP** 鈥?requiere sesion validada.
- Listado de **versiones anteriores** de cada pack, con la ultima por defecto.
- **SP Updater (auto-actualizacion)** 鈥?descarga la ultima version de la app.
- **Desactivar Nextendo** 鈥?aplica el ZIP del bot sobre la carpeta correspondiente.
- Indicador "Al dia" / "Actualizacion disponible" con verificacion real por SHA-256
  de lo que tienes instalado.
- Antes de instalar un pack puedes elegir **solo actualizar** o **limpiar e instalar**
  (limpieza que protege `emummc`, `Nintendo` y los datos de la propia app).
- Instalacion de firmware **tipo Daybreak**: valida los NCA, preserva todos los
  datos y siempre instala FAT32 + exFAT. Al terminar puedes Apagar o Reiniciar.
- Tras instalar, los ZIP descargados se eliminan de la SD.

### Guardados (Saves)
- Copia de seguridad **individual** o **de todos los juegos**, con varios perfiles
  (incluidos usuarios sin cuenta Nintendo).
- Restaurar en el usuario que elijas, eligiendo la version por fecha
  (la ultima por defecto).
- Nube de Telegram: canal privado con un **tema (topic) por usuario**, versiones
  por fecha y deduplicacion por SHA-256 (no se suben copias repetidas).
- Sin sesion: guardado anonimo identificado por el nick provisional; al iniciar
  sesion, se migran los guardados a tu cuenta y se limpia el tema viejo.
- **Saves 100% (comunidad)**: repositorio de partidas completadas (80-100%)
  visible y usable solo con sesion iniciada.
- Buscador por nombre, por letra A-Z y por usuario.
- Comprobacion de integridad de los ZIP locales.
- Borrado de guardados desde la app (version concreta, juego completo o toda la nube).
- Limites antiabuso solo para usuarios anonimos.

### Juegos
- Listado de juegos instalados y de sus partidas.
- Ver y borrar partidas por usuario o todas.
- Desinstalacion segura: juego + actualizaciones + DLC + accesos directos (forwarders)
  + datos de la SD, **conservando las partidas**.
- Limpieza quirurgica de datos huerfanos (solo titulos de juego `0100...` ya
  desinstalados; nunca toca sysmodules, overlays ni titulos de sistema).
- Eliminar actualizaciones antiguas de juegos.
- Limpiar tickets no utilizados y carpeta `erpt_reports`.
- **Crear forwarders** de cualquier NRO de `sdmc:/switch` con su propio icono;
  se instalan en el menu HOME. Detecta si ya existe un forwarder para no duplicar.

### Extras
- Interfaz tactil y navegacion con joysticks o cruceta.
- Tema blanco estilo Horizon.
- Textos legibles en los botones de ayuda (`+ Salir`, `A OK`, `B Salir`).

---

## Bot de Telegram (Raspberry Pi)

### Usuario
- `/start` 鈥?registro y menu inline con:
  - Mi codigo (usuario + ID de descarga).
  - Mis saves (lista de tus juegos en la nube).
  - Borrar mis datos (desvincular consola).
- El bot comprueba que estes en el canal de Telegram configurado.

### Administrador (todo con menus inline)
- `/start` o `/admin` 鈥?panel con opciones de usuario + administracion.
- **Ver usuarios**: listado paginado con flechas, filtro A-Z y buscador. Cada
  usuario muestra todos sus datos (Telegram ID, ID de descarga, serial, estado,
  guardados en nube, ultimo acceso y fecha de alta).
- **Bloquear / Desbloquear** acceso por usuario.
- **Borrar usuario** con doble confirmacion: borra su tema y sus partidas
  (los saves 100% de la comunidad no se tocan).
- **Juegos de un usuario**: boton por juego y listado de sus versiones, con
  opcion de ver o borrar cada una.
- **Saves 100%**: un boton por save subido; permite ver el archivo o borrarlo
  con doble confirmacion (borra tambien el mensaje de Telegram).

### Nube de guardados
- Versionado por fecha: nada se sobrescribe, siempre se puede elegir version.
- Archivos grandes troceados y recompuestos automaticamente.
- Al iniciar sesion, los guardados anonimos se trasladan fisicamente al tema del
  usuario y se elimina el tema anonimo.

---

---

## Notas

- **USB/MTP:** usa **Haze** de Atmosphere (incluido). Si Windows no lo reconoce,
  desactiva `usb30_force_enabled` en la configuracion de Atmosphere.
- **AutoRCM:** tras instalar packs o firmware, elige **Apagar** y arranca con tu
  metodo de inyeccion habitual.
- Los guardados vacios (sin datos) no se suben a Telegram.
- El bot no guarda informacion personal; solo mantiene lo necesario para
  identificar consolas y guardados.

---

## Licencias

- Interfaz: [Borealis](https://github.com/natinusala/borealis) (GPLv3).
- Partes de Atmosphere (`ams:su`, `ams:bpc`): GPLv2 / Atmosphere-NX.
- minizip, curl, mbedtls: sus respectivas licencias.
- El resto del proyecto: GPLv3.
