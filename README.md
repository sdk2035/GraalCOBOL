# GraalCOBOL 🚀

**GraalCOBOL** es una implementación de alto rendimiento del lenguaje de programación COBOL, desarrollada sobre **GraalVM** utilizando el framework Truffle.

Esta plataforma permite ejecutar aplicaciones legadas de COBOL en entornos de nube modernos con el rendimiento y las optimizaciones del compilador JIT (Just-In-Time) de GraalVM, facilitando además la interoperabilidad sin fisuras con Java y otros lenguajes del ecosistema Polyglot.

---

## 🌟 Características Principales

* **Rendimiento Avanzado (GraalVM JIT):** Compilación en tiempo de ejecución de alto nivel que optimiza rutinas pesadas de procesamiento batch y cálculo financiero.
* **Interoperabilidad Políglota:** Integración directa con librerías de Java, Python, Node.js y R a través del framework Truffle.
* **Ejecución Nativa (Native Image):** Posibilidad de compilar código COBOL a ejecutables nativos con **GraalVM Native Image** para tiempos de arranque instantáneos y menor consumo de memoria.
* **Modernización Legada:** Migración transparente de lógica de negocio crítica sin necesidad de reescribir el código base existente.

---

## 🏗️ Arquitectura de la Plataforma

* **Parser & AST Truffle:** Transforma la sintaxis estándar de COBOL en nodos de AST (Abstract Syntax Tree) optimizados para la plataforma Graal.
* **GraalVM Core Engine:** Motor de ejecución que aplica optimizaciones como *inlining*, *escape analysis* y *deoptimization* en tiempo de ejecución.
* **Polyglot API:** Interfaz para compartir memoria y variables de estructuras COBOL (ej. `01` RECORD) directamente con objetos Java o JSON.

---

## 🚦 Inicio Rápido

### Prerrequisitos

* **GraalVM JDK** (versión 21 o superior) con soporte para Truffle.
* `JAVA_HOME` configurado correctamente apuntando a la instalación de GraalVM.

### Instalación

```bash
# Clonar el repositorio
git clone [https://github.com/tu-usuario/graalcobol.git](https://github.com/tu-usuario/graalcobol.git)
cd graalcobol

# Construir el proyecto usando Gradle / Maven
./gradlew build
