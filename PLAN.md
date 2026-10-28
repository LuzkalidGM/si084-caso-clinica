# Plan de auditoría de la seguridad del canal digital de la Clínica Santa Lucía

**Universidad Privada de Tacna · Escuela de Ingeniería de Sistemas**  
**SI-084 · Auditoría de Sistemas · Examen práctico de Unidad II**

| Dato | Contenido |
| --- | --- |
| Apellidos y nombres | Gabriela Luzkalid Gutierrez Mamani |
| Código de estudiante | 2022074263 |
| URL del repositorio | https://github.com/LuzkalidGM/si084-caso-clinica/tree/examen-u2 |
| Fecha | 28/10/2026 |
| Docente | Dr. Oscar Juan Jimenez Flores |

Plan de auditoría del canal digital de la Clínica Santa Lucía. Elaborado a partir del reconocimiento de la réplica autorizada; los procedimientos se ejecutarán en la Unidad III, previa aprobación del plan y de la autorización técnica correspondiente.

## 1. Resultados del reconocimiento e información de la entidad

La Clínica Santa Lucía S.A.C. es una IPRESS privada de nivel II-1 ubicada en Tacna, supervisada por SUSALUD y por la Autoridad Nacional de Protección de Datos Personales. WebSur S.A.C. desarrolla, aloja y mantiene su portal desde 2021. La solicitud declara 3 850 citas mensuales, 12 400 pacientes con cuenta y 214 postulaciones con CV en 2026. La entidad cuenta con 162 trabajadores, tres de sistemas, y solicita evaluar la protección de los datos de salud tras el incidente del 08/09/2026.

Como parte de la planificación, la comisión ejecutó el reconocimiento autorizado del canal digital con la fase de recolección de información de la OWASP Web Security Testing Guide 4.2. El cuadro 1 contrasta lo observado con lo que declara el proveedor.

Cuadro 1. Lo declarado por WebSur S.A.C. frente a lo observado en el reconocimiento

| Prueba | Identificador WSTG | Lo observado, con cifras | Declaración | ¿Coincide? | Evidencia |
| --- | --- | --- | --- | --- | --- |
| E1 | WSTG-INFO-04 | Tres aplicaciones respondieron HTTP 200: portal en 127.0.0.1:8080, pruebas en 127.0.0.1:8081 y API en 127.0.0.1:8088. Pruebas declara una copia de producción del 12/09/2026 y publica admin_pruebas. La API anuncia api-citas 1.4.0 y entorno produccion. No se verificó el contenido de la copia. | D1 | No | E1_aplicaciones.txt |
| E2 | WSTG-INFO-02 | Las cabeceras anuncian nginx/1.20.2 y PHP/7.4.3. nginx clasifica 1.20.2 como legacy; la rama estable 1.22 la sustituyó el 24/05/2022, sin que esa fecha pruebe un fin de soporte formal. PHP 7.4 terminó su soporte el 28/11/2022. Son versiones anunciadas, no verificadas internamente. [F4–F6] | D2 | No | E2_servidor.txt |
| E3 | WSTG-INFO-03 | Cuatro rutas Disallow: /admin/, /respaldos/, /resultados/ y /wp-content/plugins/wp-file-manager/. Se anuncia un sitemap. security.txt respondió HTTP 404. robots.txt informa rutas, pero no bloquea el acceso. | — | — | E3_metarchivos.txt |
| E4 | WSTG-INFO-05 | Cuatro comentarios HTML revelan proveedor y correo, usuario admin_pruebas pendiente de eliminación, dirección interna http://10.20.0.15:8090/lis/resultados y patrón ver.php?orden=NNNNN. generator anuncia WordPress 5.8.1. Se publica contraseña inicial basada en nacimiento, ddmmaaaa. | D8 | No | E4_contenido.txt |
| E5 | WSTG-INFO-08 | Cuatro recursos anuncian MedicalPro 2.1.0, Contact Form 7 5.3.1, jQuery 3.4.1 y WP File Manager 6.8. E4 anuncia WordPress 5.8.1 frente a 6.6 declarado. La versión anunciada de File Manager cae en el rango anterior a 6.9 asociado a CVE-2020-25213. La periodicidad mensual requerirá registros; no se probó explotación. [F7] | D3 | No, con indicios; periodicidad por verificar | E5_componentes.txt |
| E6 | WSTG-INFO-06 | Cuatro formularios: citas, resultados, postulación y administración. Tres solicitan datos personales de pacientes o postulantes. Citas recoge especialidad y síntomas mediante GET; los otros tres usan POST. Postulación recibe un CV. Las cuatro direcciones action usan HTTP. No se enviaron formularios. | D7 | No | E6_entradas.txt |
| E7 | WSTG-CONF-04 · CONF-05 | /respaldos/ lista cuatro archivos: citas_export_2026-08.csv, portal_2025-09-30.sql.gz, portal_2026-03-31.sql.gz y wp-config.php.bak. Por extensión: un CSV, dos SQL comprimidos y una copia de configuración. /admin/ respondió HTTP 200 en la réplica, sin iniciar sesión ni descargar archivos. | D5 · D6 | No para D5 y D6 en la réplica | E7_expuestos.txt |
| E8 | WSTG-CONF-07 · CONF-12 · SESS-02 | Cuatro de cuatro cabeceras ausentes: HSTS, CSP, X-Frame-Options y X-Content-Type-Options. PHPSESSID no declara Secure, HttpOnly ni SameSite. HTTPS en 127.0.0.1:443 devolvió HTTP 000: no se obtuvo respuesta HTTP; el código solo no identifica la causa. | D4 | No | E8_cabeceras.txt |

Las evidencias E1 a E8 se conservarán en evidencias/ y se sellarán con SHA-256 en SHA256SUMS.txt al cerrar la entrega. Las observaciones reflejan el laboratorio en la fecha de reconocimiento, no una comprobación histórica directa del periodo auditado.

Ninguna de las ocho declaraciones D1 a D8 queda respaldada integralmente por el reconocimiento: D1, D2 y D4 a D8 presentan contradicciones; D3 muestra indicios incompatibles y exige revisar registros de actualización. La comisión no depositará confianza inicial en las afirmaciones del proveedor sin corroboración. El patrón de resultados identificado en E4 se relaciona con el incidente del 08/09/2026 y motivará pruebas de autorización y caché, sin considerar demostrada su causa. Las versiones y comentarios publicados se tratarán como indicios, no como verificación interna.

## 2. Materia de control

La comisión auditará la seguridad del canal digital de la Clínica Santa Lucía, respecto de la confidencialidad, integridad y disponibilidad de datos personales y de salud, del 01/01/2026 al 30/09/2026. La materia se justifica por el incidente del 08/09/2026, cuando una paciente visualizó un resultado ajeno, y por E4 y E6: patrón de consulta de resultados expuesto y envío previsto de síntomas mediante GET. Se priorizará este canal por su tratamiento directo de información sensible.

## 3. Alcance

IV.1 Incluye. La comisión cubrirá el portal http://127.0.0.1:8080/ y sus servicios /citas/ (3 850 citas mensuales), /resultados/ (12 400 pacientes con cuenta) y /postula/ (214 postulaciones en 2026); también /admin/, /respaldos/, metarchivos, componentes y configuraciones del canal. Incluirá el entorno de pruebas http://127.0.0.1:8081/ y la API http://127.0.0.1:8088/, no declarados en D1. La población de la API, de pruebas, de administradores y de respaldos no fue informada: se solicitarán inventarios y se conciliarán con el portal, sin duplicar los volúmenes anteriores. Los cuatro archivos listados serán unidades de revisión de ubicación y permisos, no registros de pacientes. Se revisarán las responsabilidades de WebSur, el incidente y la base legal e información al usuario de los formularios.

IV.2 Periodo auditado. La comisión evaluará del 01/01/2026 al 30/09/2026, conforme a la solicitud, para abarcar el incidente del 08/09/2026 y sus controles previos y posteriores dentro del periodo. El reconocimiento realizado el 28/10/2026 aportará indicios actuales; la comisión contrastará su continuidad con configuraciones, cambios y registros históricos. Las 214 postulaciones corresponden al año 2026 y se depurarán al periodo auditado; no se asumirá que todas pertenecen a enero–setiembre ni se multiplicarán automáticamente las citas mensuales por nueve.

IV.3 Exclusiones. Se excluirán los sistemas clínicos internos, la base de datos y el LIS de 10.20.0.15:8090 como objetivos de prueba directa, por no estar autorizados; su integración con el canal se revisará solo con documentación facilitada por Sistemas. Se excluirán equipos médicos, red corporativa, seguridad física, nómina y auditoría financiera, al no formar parte del canal solicitado. Se excluirán ingeniería social, denegación de servicio, fuerza bruta y cargas maliciosas, por innecesarias para el programa propuesto. No se accederá al dominio declarado ni a otros sistemas reales. Las pruebas de autenticación, autorización, sesión, formularios o API solo se realizarán en la Unidad III sobre la réplica y con autorización ampliada escrita.

IV.4 Limitaciones inherentes. La auditoría proporcionará seguridad razonable, no absoluta. El muestreo dirigido no permitirá inferencia estadística sobre toda la población. Una réplica con páginas estáticas puede carecer de autenticación, sesiones o API funcional; en tal caso se documentará la imposibilidad y se solicitará una réplica funcional con datos sintéticos, sin inferir eficacia del sistema real. No se acreditarán hechos históricos sin registros del periodo ni se asumirá explotabilidad por banners o versiones en URL. Las poblaciones no informadas y el acceso limitado al proveedor podrán restringir conclusiones. Se detendrá cualquier prueba que revele datos personales reales y se registrará solo la ruta.

## 4. Objetivos

Objetivo general. Determinar si los controles del canal digital protegieron adecuadamente los datos personales y de salud durante el periodo auditado, mediante su evaluación conforme a OWASP WSTG 4.2, OWASP ASVS 4.0.3, NTP-ISO/IEC 27001:2022 y los artículos aplicables de la Ley 29733.

Objetivo específico 1. Evaluar la protección de la captura y transmisión de datos y la información al usuario, conforme a WSTG-CRYP-03, ASVS 4.0.3 V9.1.1 y V8.3.1 y V8.3.3, NTP-ISO/IEC 27001:2022 A.8.24 y A.5.34, y Ley 29733, artículos 9, 13.5, 13.6 y 18, considerando las excepciones legales aplicables.

Pregunta de auditoría: ¿Los formularios protegerán los datos personales y de salud en tránsito, evitarán su inclusión en la URL y proporcionarán información y una base legal de tratamiento documentada?

Objetivo específico 2. Determinar si la autenticación, autorización y sesión impedirán el acceso a resultados ajenos y la reutilización indebida de datos en caché, conforme a WSTG-ATHZ-04, WSTG-ATHN-07 y WSTG-SESS-02; ASVS 4.0.3 V4.2.1, V8.1.1, V8.2.1, V2.3.1 y V3.4.1–V3.4.3; NTP-ISO/IEC 27001:2022 A.8.3 y A.8.5; y Ley 29733, artículos 9, 16 y 17.

Pregunta de auditoría: ¿Cada paciente accederá únicamente a sus propios resultados mediante credenciales y sesiones protegidas, incluso al alternar usuarios y revisar la caché, respondiendo al incidente del 08/09/2026?

Objetivo específico 3. Verificar la protección de activos, respaldos y administración y la gestión de componentes y entornos, conforme a WSTG-CONF-02, WSTG-CONF-04 y WSTG-CONF-05; ASVS 4.0.3 V14.2.1, V14.2.5 y V8.1.6; NTP-ISO/IEC 27001:2022 A.5.9, A.8.8, A.8.13, A.8.31 y A.8.33; y Ley 29733, artículo 16.

Pregunta de auditoría: ¿Los tres activos estarán inventariados y mantenidos, con administración y respaldos restringidos y separación y protección de pruebas frente a producción?

## 5. Enfoque, materialidad y riesgo

VI.1 Enfoque. La comisión aplicará un enfoque predominantemente sustantivo, con inspección documental, revisión de configuración y comprobaciones dirigidas en réplica, por las contradicciones del cuadro 1: activos omitidos en E1, versiones anunciadas antiguas en E2 y E5, GET y HTTP en E6, listado de respaldos en E7 y ausencia de cabeceras en E8. No se dará por eficaz un control por declaración del proveedor. Un control inexistente se registrará como brecha de diseño; solo cuando exista evidencia de su implementación se considerarán pruebas de eficacia operativa del periodo.

VI.2 Materialidad. La comisión propondrá, para priorización económica, un umbral de S/ 186 000, equivalente al 1 % de los ingresos 2025 de S/ 18 600 000 declarados en la solicitud; será un juicio de planificación sujeto a revisión según evidencia de impacto, no una pérdida observada ni una exigencia normativa. Cualitativamente, cualquier divulgación no autorizada de un resultado de salud será material, aunque afecte a una sola persona y no tenga monto estimado, por tratarse de datos sensibles y por los deberes de seguridad y confidencialidad de la Ley 29733, artículos 2.5, 9, 16 y 17. El umbral monetario no excluirá estas excepciones.

VI.3 Riesgo de auditoría. La comisión calificará el riesgo inherente como alto por los datos de salud y el incidente, junto con la exposición de varios activos observada en E1. El riesgo de control será alto por los indicios convergentes de E4, E6, E7 y E8: credenciales previsibles publicadas, envío inseguro previsto, respaldos listados y protecciones ausentes. El riesgo de detección inicial será alto por las limitaciones de una réplica y la falta de evidencia histórica; la comisión lo reducirá a un nivel planificado bajo mediante corroboración documental, revisión de configuración, cobertura total de activos y rutas críticas y pruebas dirigidas reproducibles. Si los registros o funciones no están disponibles, no declarará alcanzado ese nivel y limitará su conclusión.

## 6. Procedimientos de auditoría

La comisión ejecutará seis procedimientos en la Unidad III, previa aprobación del plan y de una autorización técnica que especifique cuentas sintéticas, operaciones y rutas habilitadas. P-01, P-02 y P-04 cubrirán OE2; P-03 cubrirá OE1; P-05 y P-06 cubrirán OE3. Se emplearán cinco categorías WSTG: ATHZ, ATHN, CRYP, SESS y CONF. El programa estimará 12 horas de ejecución, sin incluir la preparación de una réplica funcional ni la emisión del informe.

Las muestras serán dirigidas y no estadísticas. Si una población será menor que la muestra prevista, se revisará su totalidad. Se conservarán solicitudes y respuestas con datos sintéticos, configuraciones autorizadas y papeles de trabajo, sin publicar datos de pacientes ni secretos. Una función no implementada o evidencia no entregada se registrará como limitación, no como prueba superada.

### P-01 Autorización de resultados y aislamiento de caché

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | OE2 |
| Origen en el reconocimiento | E4: patrón ver.php?orden=NNNNN. E1: API de producción anunciada. Incidente del 08/09/2026. |
| Riesgo que cubre | Acceso a resultados de otro paciente por falta de autorización o reutilización indebida de caché (R1). |
| Criterio | WSTG-ATHZ-04 · OWASP ASVS 4.0.3 V4.2.1, V8.1.1 y V8.2.1 · NTP-ISO/IEC 27001:2022 A.8.3 · Ley 29733, artículos 16 y 17. |
| Tipo de prueba | Sustantiva: comprobación dirigida y revisión de configuración y registros. |
| Población y muestra | Población de referencia: 12 400 cuentas; resultados y consultas del periodo por conciliar. Muestra: dos pacientes sintéticos A y B, dos resultados por paciente y las rutas equivalentes de API expresamente autorizadas. Inspección del único expediente del incidente. |
| Evidencia a obtener | Matriz cuenta–resultado esperado/obtenido, solicitudes y respuestas sintéticas, cabeceras de caché, configuración y trazas del incidente anonimizadas. |
| Definición de excepción | Una cuenta o sesión anónima obtiene contenido de un resultado ajeno, o una caché entrega a B contenido generado para A. La ausencia de registros del incidente será limitación documental. |
| Horas estimadas | 2 horas |

**Pasos**

1. Obtenga autorización de Unidad III, la relación de propietarios de los cuatro resultados sintéticos y el expediente del incidente; identifique las rutas habilitadas, sin enumerar identificadores reales.

2. Inicie sesiones separadas de A y B. Consulte cada resultado propio como línea de base; registre propietario esperado, respuesta y cabeceras de caché.

3. Solicite desde A los dos identificadores conocidos de B, y desde B los dos de A; repita las cuatro consultas sin sesión. Verifique denegación sin contenido clínico.

4. Alterne A y B sobre la misma ruta en un navegador de prueba, cerrando sesión entre usuarios. Revise caché del navegador y configuración de caché compartida; compruebe que no reaparezcan resultados ajenos.

5. Repita solo las rutas equivalentes de API aprobadas. Compare registros del 08/09/2026 con controles de propietario y caché; no atribuya causa sin trazas. Registre excepciones o falta de funcionalidad.

### P-02 Credenciales iniciales y cuentas de prueba

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | OE2 |
| Origen en el reconocimiento | E4: contraseña inicial ddmmaaaa y comentario de eliminación de admin_pruebas. E1: usuario de pruebas publicado. |
| Riesgo que cubre | Suplantación de pacientes o uso indebido de cuentas de prueba (R2). |
| Criterio | WSTG-ATHN-07 · OWASP ASVS 4.0.3 V2.3.1 y V2.1.1 · NTP-ISO/IEC 27001:2022 A.8.5 · Ley 29733, artículo 16. |
| Tipo de prueba | Sustantiva: revisión del alta y cambio de credenciales con cuentas sintéticas. |
| Población y muestra | Población: 12 400 cuentas de pacientes; cuentas de administración y pruebas por inventariar. Muestra: dos altas sintéticas y totalidad de cuentas de prueba declaradas en el inventario autorizado. |
| Evidencia a obtener | Política y configuración de alta, dos fichas de activación sin secretos, resultado de cambio de contraseña e inventario de cuentas de prueba. |
| Definición de excepción | La fecha de nacimiento permite acceso sin cambio obligatorio; la credencial inicial no es aleatoria o carece de expiración definida; una clave elegida de menos de 12 caracteres se acepta; o una cuenta de prueba conserva acceso de producción sin justificación. |
| Horas estimadas | 1,5 horas |

**Pasos**

1. Solicite la política de autenticación y la lista autorizada de cuentas de prueba. Prepare dos pacientes sintéticos con fechas de nacimiento conocidas.

2. Solicite sus altas y observe generación, entrega y caducidad de credenciales iniciales. Revise el generador o configuración para corroborar aleatoriedad; no la infiera de dos valores.

3. Compruebe con cada cuenta si la fecha sintética publicada como clave inicial permite consultar resultados antes de un cambio obligatorio. Use solo el intento aprobado por cuenta.

4. Intente establecer una clave sintética de ocho caracteres y otra de doce; registre aceptación o rechazo. Revise que el secreto temporal no siga siendo la clave permanente.

5. Revise con el administrador la deshabilitación o aislamiento de admin_pruebas y otras cuentas de prueba. No pruebe contraseñas de usuarios reales ni use fuerza bruta. Registre cada excepción.

### P-03 Captura y transmisión de datos personales

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | OE1 |
| Origen en el reconocimiento | E6: cuatro action HTTP, citas GET con síntomas y recepción de CV. E8: HTTPS sin respuesta HTTP. Solicitud: casilla acepto sin texto. |
| Riesgo que cubre | Exposición de datos, credenciales y síntomas en tránsito o URL y tratamiento sin información suficiente (R3). |
| Criterio | WSTG-CRYP-03 · OWASP ASVS 4.0.3 V9.1.1, V8.3.1 y V8.3.3 · NTP-ISO/IEC 27001:2022 A.8.24 y A.5.34 · Ley 29733, artículos 9, 13.5, 13.6 y 18; excepciones del artículo 14 cuando correspondan. |
| Tipo de prueba | Sustantiva: inspección de cuatro flujos sintéticos, configuración TLS y avisos. |
| Población y muestra | Población: cuatro formularios; referencia de negocio de 3 850 citas/mes, 12 400 pacientes y 214 postulaciones anuales, a depurar al periodo. Muestra: totalidad de formularios, un envío sintético aprobado por cada uno y un CV de texto inocuo. |
| Evidencia a obtener | Matriz de cuatro flujos con URL, método, transporte y respuesta; configuración de TLS; avisos de privacidad y base legal documentada, sin conservar contraseñas. |
| Definición de excepción | Un flujo acepta datos sensibles o credenciales por HTTP o en la URL; o se recaban datos sin aviso previo suficiente o sin base legal aplicable documentada. Una casilla sin texto no acreditará consentimiento informado. |
| Horas estimadas | 2 horas |

**Pasos**

1. Obtenga los cuatro envíos permitidos, destinos locales y credenciales sintéticas; confirme que no se alcanzarán dominios reales ni se crearán citas o postulaciones reales.

2. En las herramientas del navegador, registre método, action y negociación TLS de cada flujo; inspeccione configuración y redirección antes de transmitir datos.

3. Envíe una vez por formulario los datos sintéticos aprobados y el CV inocuo. Compruebe que síntomas, DNI y claves no figuren en URL, historial ni parámetros, y que el transporte sea cifrado.

4. Revise aviso previo, finalidad, destinatarios, derechos y base legal del tratamiento; evalúe consentimiento escrito para datos sensibles cuando corresponda y documente cualquier excepción legal aplicable.

5. Confronte lo observado con registros de configuración y avisos del periodo. Si la réplica no procesa envíos o carece de TLS, documente la brecha o limitación sin afirmar protección en producción.

### P-04 Cookies y cabeceras de protección

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | OE2 |
| Origen en el reconocimiento | E8: PHPSESSID sin Secure, HttpOnly ni SameSite y cuatro cabeceras ausentes. |
| Riesgo que cubre | Uso indebido de sesiones y exposición del navegador a controles insuficientes (R2 y R3). |
| Criterio | WSTG-SESS-02 y WSTG-CONF-07 · OWASP ASVS 4.0.3 V3.4.1–V3.4.3 y V14.4.3–V14.4.7 · NTP-ISO/IEC 27001:2022 A.8.5 y A.8.9 · Ley 29733, artículo 16. |
| Tipo de prueba | Sustantiva: inspección de respuestas y configuración; no explotación de navegador. |
| Población y muestra | Población: páginas y cookies del canal a inventariar. Muestra dirigida: inicio, citas, resultados, postulación y administración, antes y después de autenticación donde exista; totalidad de cookies de sesión observadas. |
| Evidencia a obtener | Matriz página/estado/cookie, cabeceras de respuesta y configuración de atributos, HSTS, CSP y protección contra enmarcado. |
| Definición de excepción | Una cookie de sesión carece de Secure, HttpOnly o SameSite; una página carece de CSP o nosniff; una respuesta HTTPS carece de HSTS; o una página sensible permite enmarcado sin restricción mediante CSP frame-ancestors o X-Frame-Options. |
| Horas estimadas | 1,5 horas |

**Pasos**

1. Prepare la cuenta sintética autorizada y un navegador limpio. Inventaríe cookies de las cinco páginas sin publicar sus valores.

2. Observe Set-Cookie antes y después de iniciar sesión donde haya funcionalidad; registre Secure, HttpOnly, SameSite, dominio y ruta de cada cookie de sesión.

3. Capture cabeceras en las cinco páginas. Compruebe CSP, X-Content-Type-Options y una política contra enmarcado; evalúe HSTS solo sobre HTTPS.

4. Compare las respuestas con la configuración aprobada y sus registros de cambios. Determine si las políticas se aplican también a páginas autenticadas.

5. Registre por página el criterio incumplido. Si no hay autenticación funcional, limite la conclusión a las cookies observadas y declare pendiente la comprobación de sesiones autenticadas.

### P-05 Resguardo de respaldos y administración

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | OE3 |
| Origen en el reconocimiento | E7: cuatro archivos listados y /admin/ HTTP 200. E3: rutas sensibles publicadas. |
| Riesgo que cubre | Exposición de respaldos y configuración o acceso indebido a administración (R4). |
| Criterio | WSTG-CONF-04 y WSTG-CONF-05 · OWASP ASVS 4.0.3 V8.1.6 y V4.1.1 · NTP-ISO/IEC 27001:2022 A.8.13 y A.8.3 · Ley 29733, artículos 16 y 17. |
| Tipo de prueba | Sustantiva: revisión de rutas, permisos y restricciones de administración. |
| Población y muestra | Población: cuatro archivos listados, un directorio /respaldos/ y una interfaz /admin/; inventario histórico de respaldos por solicitar. Muestra: totalidad de esos elementos y dos perfiles de red simulados, autorizado y no autorizado. |
| Evidencia a obtener | Matriz de ubicación/permisos de los cuatro archivos, reglas del servidor y pruebas de acceso desde dos perfiles locales; política y responsables de respaldo. |
| Definición de excepción | El usuario público puede listar nombres de respaldos o leerlos según permisos; un respaldo reside bajo la raíz pública sin bloqueo; o la interfaz administrativa es alcanzable desde el perfil de red no autorizado. HTTP 200 no demostrará autenticación superada. |
| Horas estimadas | 2 horas |

**Pasos**

1. Solicite el inventario de respaldos, responsables, ubicación y reglas de acceso del periodo. Identifique los cuatro nombres de E7 sin descargar sus contenidos.

2. Revise con el administrador las rutas físicas y permisos del servidor en la réplica; determine si el usuario web podría leer los archivos y si están bajo la raíz pública.

3. Compruebe el listado de /respaldos/ desde el perfil público autorizado y contraste el resultado con las reglas; no descargue los archivos.

4. Desde dos perfiles de red simulados y aprobados, solicite /admin/ sin credenciales. Verifique la restricción al perfil autorizado mediante respuesta y registros de red.

5. Revise autorización y necesidad de la copia wp-config.php.bak y documente responsables del respaldo y excepciones. No equipare presencia de login con acceso administrativo obtenido.

### P-06 Componentes y separación de entornos

| Elemento | Contenido |
| --- | --- |
| Objetivo específico | OE3 |
| Origen en el reconocimiento | E1: tres activos frente a uno declarado y comentario de copia de producción. E2 y E5: versiones anunciadas antiguas. E4: información interna y cuenta de pruebas. |
| Riesgo que cubre | Compromiso por componentes vulnerables y exposición de datos mediante entornos no controlados (R5). |
| Criterio | WSTG-CONF-02 · OWASP ASVS 4.0.3 V14.2.1, V14.2.5 y V14.3.3 · NTP-ISO/IEC 27001:2022 A.5.9, A.8.8, A.8.31 y A.8.33 · Ley 29733, artículos 6 y 16. |
| Tipo de prueba | Sustantiva: inspección de inventario, versiones internas y configuración; sin ejecutar exploits. |
| Población y muestra | Población: tres activos y componentes instalados por inventariar. Muestra: los tres activos, siete versiones anunciadas (API, nginx, PHP, WordPress, tema, Contact Form 7 y jQuery) más WP File Manager, y totalidad de los cambios del periodo relativos a esos ocho componentes. |
| Evidencia a obtener | Inventario conciliado, versiones internas y soporte oficial, análisis de aplicabilidad de CVE-2020-25213, historial de cambios, reglas de separación y constancia de anonimización o datos sintéticos. |
| Definición de excepción | Un activo carece de responsable e inventario; un componente instalado está sin soporte o tiene vulnerabilidad aplicable sin tratamiento documentado; pruebas es accesible sin necesidad autorizada o conserva datos identificables de producción sin protección y base legal; o se exponen versiones detalladas. |
| Horas estimadas | 3 horas |

**Pasos**

1. Obtenga el inventario interno y concilie portal, pruebas y API con E1. Asigne responsables y documente toda diferencia respecto de D1.

2. Verifique internamente las ocho versiones anunciadas mediante inventario o configuración facilitada; no dé por instalado un producto únicamente por una URL o cabecera.

3. Compare versiones verificadas con soporte y avisos oficiales. Evalúe CVE-2020-25213 solo si confirma producto, versión y condiciones; no ejecute código de explotación.

4. Examine cambios y actualizaciones del periodo para comprobar D3. Compare separaciones de red, credenciales y permisos de pruebas y producción y revise eliminación de comentarios y banners innecesarios.

5. Solicite constancia de origen y anonimización de los datos de pruebas. Revise metadatos y controles con Sistemas, sin copiar registros personales. Registre excepciones y, si faltan pruebas históricas, limite la conclusión al estado comprobado.

## 7. Anexo A Matriz de riesgos

| N.º | Riesgo | Causa, con su evidencia | Impacto | Nivel | Control existente | Control esperado |
| --- | --- | --- | --- | --- | --- | --- |
| R1 | Divulgación de resultados ajenos | E4 revela un identificador de orden; el incidente acredita una reclamación. La falla de autorización o caché es hipótesis por verificar. | Pérdida de confidencialidad de salud. | Alto | Formulario de acceso visible; eficacia no probada. | Autorización por propietario en cada consulta y API; aislamiento de caché; trazabilidad. P-01. |
| R2 | Suplantación o uso indebido de sesión | E4 publica clave inicial basada en nacimiento; E8 muestra cookie sin Secure, HttpOnly ni SameSite. | Acceso a cuentas y resultados. | Alto | Campos de DNI/clave y cookie; no acreditan autenticación segura. | Credenciales iniciales aleatorias y temporales; cambio obligatorio; cookies protegidas. P-02 y P-04. |
| R3 | Exposición en captura y tránsito | E6: citas GET, síntomas y cuatro action HTTP; E8: cuatro cabeceras ausentes. Solicitud informa acepto sin texto. | Divulgación de datos y tratamiento sin información suficiente. | Alto | POST en tres formularios y casilla acepto; no garantizan cifrado ni consentimiento. | TLS; datos sensibles fuera de URL; aviso y base legal documentados; cabeceras adecuadas. P-03 y P-04. |
| R4 | Exposición de respaldos y administración | E7: cuatro archivos listados y /admin/ HTTP 200. E3 anuncia rutas sensibles. | Divulgación potencial de datos o configuración; acceso administrativo indebido. | Alto | Formulario de administración; no se observó restricción de red. | Respaldos fuera de raíz pública, acceso restringido, responsables y pruebas de restauración; administración limitada. P-05. |
| R5 | Componentes y entornos sin control | E1: pruebas y API no declaradas, copia de producción anunciada; E2/E5: versiones antiguas y CVE potencial. | Compromiso del canal y exposición de datos en pruebas. | Alto | Proveedor mantiene el portal; no se acreditaron registros de actualización ni separación. | Inventario de tres activos, componentes soportados, tratamiento de vulnerabilidades, separación y datos sintéticos. P-06. |

Elaborado por: Gabriela Luzkalid Gutierrez Mamani · Código: 2022074263 · Fecha: 28/10/2026.

## Fuentes y precisiones de los criterios

F1. Caso académico: SOLICITUD-DE-AUDITORIA.md, INVENTARIO-DECLARADO.md y ALCANCE-AUTORIZADO.md; evidencias E1 a E8. Volúmenes declarados, no verificados mediante extracción de registros.

F2. OWASP WSTG 4.2: <https://wstg.owasp.org/v4.2/> . Los procedimientos usan ATHZ-04, ATHN-07, CRYP-03, SESS-02 y CONF-02/04/05/07. Se conserva CONF-12 en el cuadro por fidelidad a la plantilla del examen; no se usa como criterio del programa porque no figura en el capítulo 4.2 de la edición 4.2 consultada.

F3. OWASP ASVS 4.0.3: <https://github.com/OWASP/ASVS/tree/v4.0.3/4.0/en> . NTP-ISO/IEC 27001:2022, anexo A, como marco de criterios del ejercicio; su uso no supone certificación ni obligatoriedad contractual acreditada. Referencia internacional: <https://www.iso.org/standard/27001> . Ley 29733: <https://www.gob.pe/institucion/congreso-de-la-republica/normas-legales/243470-29733> .

F4. PHP, ramas sin soporte: <https://www.php.net/eol.php> . PHP 7.4: fin de soporte oficial 28/11/2022.

F5. nginx, versiones: <https://nginx.org/en/download.html> . nginx 1.20.2 figura como legacy.

F6. nginx, historial: <https://nginx.org/en/CHANGES-1.22> . Rama estable 1.22.0 publicada el 24/05/2022; no se confundirá esta fecha con una declaración formal de fin de soporte de 1.20.

F7. CVE-2020-25213: <https://github.com/advisories/GHSA-w774-7g7w-83fx> . Afecta a WP File Manager anterior a 6.9; la correspondencia de la versión publicada en E5 es indicio y no explotación demostrada.

Las pruebas actuales se limitaron al reconocimiento. La comprobación HTTPS de E8 en 443 proviene de la orden expresa del examen, aunque el alcance general enumera 8080–8090; la comisión aclarará esta discrepancia con el docente antes de ampliar cualquier prueba. No se considerará HTTP 000 prueba suficiente de una causa concreta.

