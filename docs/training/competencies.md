# Capacitación y evidencias de competencia

**Estado:** programa propuesto. Las horas de laboratorio y las competencias demostradas se registran por separado de los años de experiencia laboral declarados. Completar estos ejercicios no se convierte automáticamente en años de experiencia.

## 1. Matriz de los seis temas solicitados

| Tema | Práctica y referencia | Entorno mínimo | Evidencia de evaluación |
| --- | --- | --- | --- |
| Desarrollo y mantenimiento COBOL | Diagnosticar y adaptar PAYROLL; copybooks, cálculos, tests y cambio trazable con Rascal | L0 para variante portable; L2 para toolchain original | Diff revisado, casos límite, resultados y explicación del cambio |
| Batch y online CICS/otros monitores | Nómina por job; CASH00 para solicitudes A/Q/U/X/C/D y fallos transaccionales | L1 compatible para batch; L2 para CICS real | Spool/RC y trazas online con consistencia de saldo y recuperación |
| JCL y Mainframe/AS400 | JOB/EXEC/DD, PROC, parámetros, DISP/DCB, COND/IF, reinicio; ruta IBM i con CL/jobs | L1/L2 para JCL; L3 para IBM i | Jobs correctos e inducidos a fallo; diagnóstico y reinicio; evidencia IBM i separada |
| SQL: consultas, joins, subconsultas, DML | Esquema sintético de empleados/cuentas; SELECT, JOIN, EXISTS, agregación, INSERT/UPDATE/DELETE y rollback | L0 con base relacional; Trino para lectura analítica | SQL versionado, resultados, plan y demostración de efectos/rollback en el motor elegido |
| DB2 u otras bases relacionales | DDL, claves, índices, restricciones, aislamiento y deadlock; especificar Db2 z/OS, LUW, i o motor alternativo | L2 para DB2 z/OS; L0/L3 según variante | DDL ejecutado, planes, errores, concurrencia y recuperación |
| COBOL con bases de datos / Embedded SQL | CASH00, SQLCA, variables host, indicadores, SELECT INTO, precompilación y bind | L2 para flujo original; otro toolchain explicitado para alternativa | Listado/precompile, DBRM/paquete/plan cuando corresponda, ejecución y manejo de errores |

Para **joins y subconsultas**, crear ejercicios adicionales: no están demostrados por el simple CRUD de CASH00. Para **cursores**, NULL y COMMIT/ROLLBACK, ampliar el caso con especificación y prueba; no afirmar que la referencia ya los resuelve.

## 2. Secuencia de ejercicios

1. **Diagnóstico inicial:** leer una rutina y un job, identificar layout y anticipar resultados. Registrar experiencia declarada por tema.
2. **PAYROLL-BATCH:** preparar datos por columnas, corregir problemas detectados en una copia de laboratorio, compilar/enlazar y ejecutar. Comparar con un oráculo calculado y luego capturado del runtime validado.
3. **JCL-OPERATIONS:** allocation, concatenación, PROC, DISP, COND, IF y reinicio. Usar Mastering JCL como guía revisada y comprobar comportamiento en el sistema invitado disponible.
4. **CASH-ONLINE:** invocar alta/consulta/actualización/baja/crédito/débito; probar cuenta inexistente, entradas inválidas, límite de COMMAREA y solicitudes concurrentes.
5. **SQL-EMBEDDED:** reconciliar DDL/DCLGEN/código, precompilar y bind; manejar no encontrado, NULL, duplicado y rollback sin presentar JDBC como sustituto de precompilación.
6. **MIGRATION:** generar inventario y dependencias con Rascal; migrar primero una rutina pura de cálculo. El programa completo espera las capacidades de archivos, control y transacciones.
7. **DATA-ANALYTICS:** exportación validada, lectura Cobrix, publicación Iceberg y consulta Trino con joins contra datos sintéticos; comprobar totales de nómina y saldo.
8. **OPERATIONS:** enviar desde el portal inspirado en WebJCL, revisar spool y recuperar un fallo sin duplicar pagos.
9. **IBM-I:** práctica específica de jobs/CL, bibliotecas y Db2 for i, o marcarla pendiente si no hay acceso. No sustituirla por terminal 3270.

Las tasas PF/impuestos y reglas de cuentas pertenecen a los ejemplos didácticos; no son reglas fiscales o bancarias listas para uso productivo.

## 3. Rúbrica por competencia

| Nivel | Evidencia |
| --- | --- |
| 0 — No evaluado | No hay ejercicio o entorno disponible |
| 1 — Guiado | Completa un caso con instrucciones y explica entradas/salidas |
| 2 — Autónomo | Resuelve variaciones y diagnostica un error sin solución preparada |
| 3 — Mantenimiento | Modifica el caso, preserva contratos y aporta regresión/reinicio |
| 4 — Integración | Migra una unidad, valida datos/semántica y demuestra operación y retorno |

El nivel es interno a este programa. Una evaluación de CICS requiere evidencia CICS; un simulador de transacciones genérico se registra con su nombre. Análogamente, practicar PostgreSQL no acredita experiencia específica en DB2.

## 4. Ficha propuesta de experiencia y evaluación

Campos por cada uno de los seis temas:

- `experienciaLaboralAniosDeclarados`: valor informado por la persona, opcional; nunca calculado desde el laboratorio.
- `plataformaYVersion`: z/OS, IBM i, emulación histórica, Linux, motor SQL y herramientas utilizadas.
- `periodosYContexto`: fechas y descripción de proyectos aportadas por la persona.
- `horasPracticaRegistradas`: duración del laboratorio en una unidad independiente.
- `ejerciciosSuperados`, `nivelDemostrado`, `limitaciones`.
- `evidencias`: commits, hashes de corpus, spool, informes de comparación y fecha.
- `evaluador` y `fechaRevision`.

No sumar periodos laborales superpuestos al calcular una duración revisada ni convertir autoevaluación en validación automática. Mostrar “sin evidencia” cuando corresponda.

## 5. Requisitos para publicar una práctica

La ficha del ejercicio debe identificar repositorio/SHA, cambios didácticos, perfil mínimo, capacidades, datos sintéticos, resultado esperado, criterio de aprobación, limpieza y limitaciones. Los hallazgos de las [referencias](../references/projects.md) se resuelven antes de declarar un corpus como oráculo. El portal conserva evidencias por alumno con acceso y retención definidos.

[Laboratorio](mainframe-lab.md) · [Plan de migración](../migration/roadmap.md) · [README](../../README.md)
