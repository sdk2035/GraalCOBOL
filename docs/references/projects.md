# Referencias de integración y hallazgos de revisión

**Revisión:** 7 de octubre de 2026. Se inspeccionaron árboles y archivos de los seis repositorios suministrados; no se ejecutaron sus programas ni se modificaron esos repositorios. Los cambios de este PR pertenecen solamente a GraalCOBOL.

## 1. Registro de referencias

| Repositorio y snapshot | Origen del fork informado por GitHub | Papel en GraalCOBOL |
| --- | --- | --- |
| [cobrix](https://github.com/sdk2035/cobrix/tree/8750ebb6d962333cf6ae9477e1b3e475eda59175) | AbsaOSS/cobrix | Decodificación de copybooks y datos; ingesta y posible conector directo Trino |
| [hercules](https://github.com/sdk2035/hercules/tree/1a40566d755a43ce9c610caaf566484a3809da0b) | opensourcemainframes/hercules | Base para estudiar emulación y migración del laboratorio a Hyperion |
| [Payroll-Processing-System](https://github.com/sdk2035/Payroll-Processing-System/tree/42f9610d315d71a5e93b7f95451eecaac3b2e63b) | dhyey2503/Payroll-Processing-System | Caso batch de nómina, copybook, archivos y jobs |
| [Mastering JCL](https://github.com/sdk2035/Mastering-JCL-A-Complete-Mainframe-Reference./tree/87634f5c6b84b46fef0e0de61831238be1eeda03) | vishak14/Mastering-JCL-A-Complete-Mainframe-Reference | Referencia curricular de JCL y ejercicios de control/datasets |
| [cash-account-cobol](https://github.com/sdk2035/cash-account-cobol/tree/a13c7ca5d3f2a8f91849deb966664a1083a9feec) | IBMStockTrader/cash-account-cobol | Caso online CICS, DB2, SQL embebido y VSAM |
| [webjcl](https://github.com/sdk2035/webjcl/tree/3df5879279acdcd608884eceab8b693d8418751f) | niumainframe/webjcl | Patrón de portal, envío de jobs y recuperación de resultados |

Las tres últimas referencias se incorporan como herramientas/materiales complementarios: guía JCL, aplicación CICS/DB2 y portal web. Ninguna de ellas se interpreta como un migrador COBOL completo. Las licencias y avisos de cada snapshot deben verificarse antes de copiar código o distribuir una imagen; por ahora se enlazan y se documentan sus interfaces.

## 2. Cobrix: datos, no ejecución de programas

Se revisaron README y `build.sbt`: el parser de copybooks está separado de la integración Spark. El build contiene variantes Scala; seleccionar el artefacto por compatibilidad probada con el consumidor, no solo por el número de versión más reciente.

No usar Cobrix como sustituto del frontend PROCEDURE DIVISION de Rascal. Comparar layouts entre ambos cuando compartan copybooks. Las rutas lakehouse y conector Trino están detalladas en [Cobrix–Trino](../architecture/cobrix-trino.md).

## 3. Hercules: fijar variante antes de migrar

Se revisaron `configure.ac` y `RELEASE.NOTES`; sus identificadores de versión no permiten atribuir al fork una release SDL Hyperion actual. Conservar snapshot base y contrastarlo con una versión elegida del upstream Hyperion. El [plan del laboratorio](../training/mainframe-lab.md) separa actualización del emulador, estado del invitado y migración de aplicaciones.

## 4. Payroll: corpus batch con correcciones previas

Se revisaron [PAYROLL.cbl](https://github.com/sdk2035/Payroll-Processing-System/blob/42f9610d315d71a5e93b7f95451eecaac3b2e63b/COBOL/PAYROLL.cbl), [EMPREC.cpy](https://github.com/sdk2035/Payroll-Processing-System/blob/42f9610d315d71a5e93b7f95451eecaac3b2e63b/COPY/EMPREC.cpy), los cuatro jobs y el reporte de muestra.

| Hallazgo estático | Implicación para el ejercicio |
| --- | --- |
| El programa declara `01 EMP-REC` antes de COPY y el copybook contiene otro `01 EMP-REC` | Revisar expansión y estructura con el compilador elegido; no usar el ejemplo como prueba de parsing correcto sin compilar |
| El código usa archivos `ORGANIZATION IS SEQUENTIAL` | No anunciar este caso como prueba de acceso VSAM indexado |
| Los datos en LOADIN deben ocupar offsets exactos del copybook | Generar un fixture por columnas de 80 bytes y validar cada campo antes de calcular |
| `OT-RATE` vale 100; el reporte de muestra no concuerda con todas las fórmulas/entradas pretendidas | Recalcular el oráculo y conservar el reporte original como referencia no validada |
| Jobs fijan HLQ `Z79184` y biblioteca de compilador `IGYV6R30.SIGYCOMP` | Parametrizar para el entorno; un invitado histórico no garantiza ese compilador |
| No hay implementación de SQL/CICS en el árbol examinado | Añadir esas competencias mediante el caso Cash y ejercicios específicos |

Ejemplo de comprobación aritmética, **suponiendo** que los campos pretendidos de la primera fila son salario 50 000, horas extra 10 e impuesto 10%: el código produce bruto 51 000, PF 6 000, impuesto 5 100 y neto 39 900. La muestra indica bruto 60 000 y neto 48 000. Esto es una discrepancia estática, no un resultado de ejecución; antes hay que resolver formato y compilación.

El programa completo requiere COPY, COMP-3, archivos, PERFORM, COMPUTE, campos editados y GOBACK; queda fuera del primer subconjunto H2. Empezar por una rutina de cálculo reducida y marcarla como variante didáctica, conservando la trazabilidad.

## 5. Mastering JCL: guía que requiere contraste

El [README examinado](https://github.com/sdk2035/Mastering-JCL-A-Complete-Mainframe-Reference./blob/87634f5c6b84b46fef0e0de61831238be1eeda03/README.md) cubre JOB/EXEC/DD, datasets, procedimientos, control condicional, utilidades, VSAM y diagnóstico. El árbol es una referencia documental, no un motor que ejecute JCL.

La tabla de COND describe comparaciones verdaderas como ejecución del paso. Para el parámetro EXEC COND, una comparación verdadera **omite** el paso. El material didáctico derivado debe corregirlo y contrastarlo con [IBM: parámetros EXEC](https://www.ibm.com/docs/en/zos-basic-skills?topic=do-jcl-exec-statements-positional-frequently-used-parameters).

Ejercicio propuesto, con un único paso previo STEP1 que termina normalmente:

| Parámetro del paso siguiente | STEP1 RC=0 | STEP1 RC=4 |
| --- | --- | --- |
| `COND=(0,EQ,STEP1)` | Omite | Ejecuta |
| `COND=(0,NE,STEP1)` | Ejecuta | Omite |

Tratar por separado abend y otras condiciones del job; esa tabla no cubre todas las reglas de ejecución. Probar los ejemplos en el JES invitado en lugar de memorizar una tabla sin contexto.

## 6. Cash Account: integración empresarial y Embedded SQL

Se revisaron [CASH00.cbl](https://github.com/sdk2035/cash-account-cobol/blob/a13c7ca5d3f2a8f91849deb966664a1083a9feec/COBOL/CASH00.cbl), ambos DCLGEN, DDL y bind. El README describe exposición mediante z/OS Connect y una cadena de identidad; este diseño conserva ese patrón como referencia, sin afirmar que esa infraestructura esté disponible.

Hallazgos que deben convertirse en tareas de preparación del laboratorio:

- `DB2DDL.jcl` declara `cyrrnbase`, mientras DCLFRANK y SQL usan `CURRNBASE`: reconciliar DDL, DCLGEN y código.
- El bind nombra `MEMBER(ACCT01)`, pero el programa examinado es `CASH00`: documentar y comprobar la relación real entre programa, DBRM y paquete.
- La DDL incluye permisos amplios a PUBLIC: sustituirlos por roles del laboratorio antes de ejecutarla en un entorno compartido.
- Verificar longitud de COMMAREA/EIBCALEN, diferencias de longitud de nombres y parámetros inválidos antes de copiar al layout interno.
- Probar NULL en columnas anulables y el uso de indicadores; las lecturas revisadas no aportan una política completa.
- Crédito/débito realizan lectura y posterior actualización: estudiar concurrencia y pérdida de actualizaciones. La lectura del tipo de cambio necesita verificar su propio SQLCODE antes de calcular.
- Registrar los resultados SQL y CICS por separado; SQLCODE no describe el resultado de WRITE sobre HISTORY.

El caso permite diseñar pruebas de CRUD, precisión, coexistencia y fallos, pero no demuestra que el runtime GraalCOBOL implemente CICS/DB2. La estrategia inicial es integrar el servicio legado autorizado; portar su lógica requiere capacidades y evidencias posteriores.

## 7. WebJCL: patrón de portal, actualización necesaria

Se revisaron README, INSTALL, MAINTENANCE, `http/JobsApi.js` y `framework/JESWorker/ISrcProc.js`. La API existente ofrece listado, envío y consulta; el diseño nuevo añadirá control de cancelación, idempotencia y autorización por recurso. No se afirma que estas mejoras ya existan.

La documentación de instalación es histórica y la nota de mantenimiento reconoce almacenamiento de todos los envíos. Modernizar dependencias, transporte, identidad, aislamiento y retención como parte de una entrega separada del portal; no copiar sus comandos de limpieza destructiva como procedimiento automático de capacitación.

## 8. Entregables derivados

| Entregable futuro | Referencias | Puerta de aceptación |
| --- | --- | --- |
| Corpus batch corregido | Payroll + Mastering JCL | Compila en perfil elegido y concilia entrada/reporte/RC |
| Caso online de referencia | Cash Account | DDL/copybooks/bind consistentes, errores y concurrencia evaluados |
| Portal de prácticas | WebJCL | Envío/consulta/cancelación aislados y reintento sin duplicados |
| Perfil Hyperion | Hercules + upstream seleccionado | IPL/job/spool/exportación/restore repetibles |
| Ruta analítica | Cobrix + Trino | Mismo conteo, claves y valores exactos que la extracción de referencia |
| Evidencia de competencias | Las seis referencias | Rúbrica por tema y plataforma, separada de años laborales |

[Capacitación](../training/competencies.md) · [Arquitectura](../architecture/integration.md) · [README](../../README.md)
