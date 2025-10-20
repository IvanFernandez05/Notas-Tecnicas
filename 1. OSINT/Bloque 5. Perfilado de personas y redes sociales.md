#### Objetivo del perfilado

Construir un perfil digital de una persona objetivo con datos públicos: nombres, alias, correos, empleos, relaciones, ubicaciones y huellas en redes.

#### Fuentes principales

- LinkedIn (trayectoria profesional).
- Twitter/X (opiniones, relaciones, geolocalizaciones).
- Facebook / Instagram (fotos, eventos).
- Repositorios (GitHub) y foros técnicos.
- Bases de datos de filtraciones (HaveIBeenPwned), usar hashes/hashes ya públicos, respetando la ley.

#### Técnicas de correlación

- **Cross-linking**: mismo nickname en distintas plataformas = probable misma persona.
- **Email pivoting**: buscar un correo en GitHub, LinkedIn o commits.
- **Phone / number reverse lookup**.

#### Búsqueda de usernames (alias)

- Herramientas: SherlocK, WhatsMyName, búsquedas manuales.
- Reglas: probar variaciones (puntos, guiones, sufijos numéricos).

#### Búsqueda inversa de imágenes & análisis de imágenes

- Motores: Google Images, Bing, TinEye.
- Extraer metadata EXIF (coordenadas GPS, timestamp, cámara).
- Analizar contenido: señales de ubicación (arquitectura, letreros, vegetación).

#### Geolocalización a partir de imágenes

- Identificar pistas (placas, anuncios, arquitectura).
- Usar mapas, Street View y herramientas de geoestimación para correlacionar.

#### Privacidad, ética y legalidad

- Evitar divulgar información sensible sin consentimiento.
- Documentar fuentes y fechas.
- Respetar leyes de protección de datos y evitar daño a personas.


