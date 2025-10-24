# RegresionLinealApp

Aplicación web en Kotlin que permite ingresar puntos (x, y), calcular la regresión lineal por mínimos cuadrados y visualizar tanto la ecuación \(y = m x + b\) como el gráfico de puntos y la recta ajustada. Está implementada con Ktor (Netty) y sirve un frontend estático sencillo desde el propio backend.

## Tabla de contenidos
- Descripción
- Requisitos
- Instalación y ejecución
- Uso de la aplicación
- Validación y manejo de errores
- Detalles de implementación
- Pruebas sugeridas
- Estructura del proyecto
- Preguntas
- Licencia

## Descripción
El backend expone un endpoint POST /regresion que recibe una lista JSON de puntos {x, y}, valida la entrada y devuelve pendiente m, intercepto b, coeficiente de determinación R² y predicciones \(\hat{y}\). El frontend (index.html) permite pegar los puntos, enviar la petición y ver el resultado y el gráfico en el navegador.

## Requisitos
- Java 17 (JDK) instalado y en PATH (java -version debe mostrar 17.x). No necesitas tener Gradle instalado porque el proyecto incluye Gradle Wrapper7.

## Instalación y ejecución
Opción A: Gradle Wrapper (recomendado)
- Windows: abrir Terminal/Powershell en la raíz del repo y ejecutar:
  - .\gradlew.bat :backend:run
- Linux/macOS:
  - chmod +x ./gradlew (si es necesario)
  - ./gradlew :backend:run
- Navega a http://localhost:8080 para abrir la UI .

Opción B: ZIP ejecutable sin Gradle
- Generar distribución:
  - ./gradlew :backend:distZip (o .\gradlew.bat :backend:distZip en Windows)
- Descomprimir backend/build/distributions/backend.zip
- Ejecutar:
  - Linux/macOS: bin/backend
  - Windows: bin\backend.bat
- Abre http://localhost:8080.

Opción C: JAR “fat” (todo en uno)
- Añadir el plugin Shadow y ejecutar:
  - ./gradlew :backend:shadowJar
- Ejecutar:
  - java -jar backend/build/libs/backend-all.jar
- Abre http://localhost:8080 .

Configuración del puerto
- Por defecto 8080. Puedes cambiarlo con:
  - ./gradlew :backend:run --args="-port=9090"
  - o con Java: java -jar ... -DPORT=9090
- Los argumentos de run se pasan con --args= según la guía del Application plugin .

## Uso de la aplicación
- Ingresa puntos como líneas con “x,y”, por ejemplo:
  1,2
  2,3
  3,3.5
- Presiona “Calcular”. Se mostrará la ecuación “y = m x + b”, el valor de R² y se dibujarán los puntos y la recta ajustada en el gráfico.

## Validación y manejo de errores
- Se requieren al menos 2 puntos válidos para calcular la recta.
- Si el JSON es inválido o hay valores no numéricos, el backend responde 400 con un mensaje claro.
- Si la varianza de X es cero (todos los x iguales), se devuelve 400 indicando que no puede ajustarse una recta con pendiente definida .

## Detalles de implementación
- Framework: Ktor (Netty) con ContentNegotiation y Gson para JSON.
- Endpoint: POST /regresion
  - Entrada: JSON de lista de objetos { "x": number, "y": number }.
  - Salida: { m, b, r2, predicciones, n }.
- Cálculo por mínimos cuadrados:
  - Pendiente: \( m = \frac{N \sum xy - \sum x \sum y}{N \sum x^{2} - (\sum x)^{2}} \)
  - Intercepto: \( b = \frac{\sum y - m \sum x}{N} \)
  - Ecuación: \( y = m x + b \)
  - R²: \( 1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2} \) .

## Pruebas sugeridas
- Entradas válidas:
  - Puntos claramente lineales (p. ej., y = 2x + 1) para verificar que m≈2 y b≈1.
  - Puntos con leve ruido para ver R² entre 0 y 1.
- Casos inválidos:
  - Lista vacía o un solo punto → 400.
  - Texto en lugar de números → 400.
  - X constantes (misma x en todos los puntos) → 400 .

## Estructura del proyecto
- Raíz (Gradle Wrapper): gradlew, gradlew.bat, gradle/wrapper/*
- settings.gradle.kts: rootProject.name e include(":backend")
- backend/build.gradle.kts: plugins Kotlin JVM + application, dependencias Ktor + Gson
- backend/src/main/kotlin/com/ana/regresion/Main.kt: servidor Ktor, ruta estática y POST /regresion
- backend/src/main/resources/static/index.html: UI .


## Preguntas 
- ¿Necesito Gradle instalado? No. Usa los scripts gradlew/gradlew.bat incluidos (Gradle Wrapper) .
- ¿Puedo cambiar el puerto? Sí, con --args en Gradle o -DPORT en Java.
- ¿Dónde está el frontend? En backend/src/main/resources/static/index.html, servido por Ktor .

## Autores

**Ana María Hernández Zea / Johan Steven Galeano Gonzalez**  
Estudiantes de Ingeniería en Ciencias de la Computación e Inteligencia Artificial.  
Universidad Sergio Arboleda.
