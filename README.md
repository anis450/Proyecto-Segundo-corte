#  Lineal Regression App – Kotlin + Spring Boot

Aplicación web desarrollada en **Kotlin** usando el framework **Spring Boot**, que permite al usuario realizar una **regresión lineal simple** ingresando sus propios datos.
El proyecto combina un backend en Kotlin con una interfaz web sencilla en HTML + JavaScript.



##  Características principales

* Permite ingresar valores de **X** y **Y** desde un formulario web.
* Calcula la **pendiente (slope)** y el **intercepto** de la recta de regresión.
* Implementa la fórmula de regresión lineal simple.
* Desarrollada con **Spring Boot 3**, **Kotlin** y **Maven**.
* Se ejecuta en el puerto **8080** (por defecto).



## 🧩 Estructura del proyecto

```
lineal-regression-app/
├── src/
│   ├── main/
│   │   ├── kotlin/com/lineal/regression/app/
│   │   │   ├── LinealRegressionAppApplication.kt   # Punto de entrada
│   │   │   ├── controller/RegressionController.kt  # Controlador REST
│   │   │   ├── service/RegressionService.kt        # Lógica de regresión
│   │   │   └── model/                             # Clases de datos
│   │   └── resources/
│   │       ├── static/index.html                   # Interfaz web
│   │       └── application.properties              # Configuración
├── pom.xml                                          # Dependencias Maven
```



##  Configuración del entorno

### Requisitos previos

* **IntelliJ IDEA** (versión Community o Ultimate)
* **JDK 17 o 21** (recomendado: Microsoft OpenJDK o Amazon Corretto)
* **Maven** (se instala automáticamente con IntelliJ)



##  Explicación del código

### `LinealRegressionAppApplication.kt`

Archivo principal que arranca la aplicación:

```kotlin
@SpringBootApplication
class LinealRegressionAppApplication

fun main(args: Array<String>) {
    runApplication<LinealRegressionAppApplication>(*args)
}
```

🔹 Inicializa Spring Boot y levanta el servidor embebido **Tomcat** en el puerto `8080`.



### `RegressionController.kt`

Define el endpoint que recibe los datos del usuario:

```kotlin
@RestController
@RequestMapping("/api/regression")
class RegressionController(val regressionService: RegressionService) {

    @PostMapping
    fun calculateRegression(@RequestBody data: RegressionRequest): RegressionResponse {
        return regressionService.calculateRegression(data)
    }
}
```

🔹 Recibe los valores `x` y `y` en formato JSON y llama al servicio matemático.



### `RegressionService.kt`

Implementa la **lógica de regresión lineal simple**:

```kotlin
@Service
class RegressionService {

    fun calculateRegression(data: RegressionRequest): RegressionResponse {
        val n = data.x.size
        val meanX = data.x.average()
        val meanY = data.y.average()

        val numerator = data.x.zip(data.y).sumOf { (xi, yi) -> (xi - meanX) * (yi - meanY) }
        val denominator = data.x.sumOf { (it - meanX).pow(2) }

        val slope = numerator / denominator
        val intercept = meanY - slope * meanX

        return RegressionResponse(slope, intercept)
    }
}
```

🔹 Calcula la pendiente (`slope`) y el intercepto (`intercept`) de la recta.



### `model/RegressionRequest.kt` y `RegressionResponse.kt`

Definen el formato de los datos que se envían y reciben:

```kotlin
data class RegressionRequest(val x: List<Double>, val y: List<Double>)
data class RegressionResponse(val slope: Double, val intercept: Double)
```


### `index.html`

Interfaz web simple para el usuario:

```html
<form id="regression-form">
  <label>X values:</label>
  <input type="text" id="x" placeholder="1,2,3,4">
  <label>Y values:</label>
  <input type="text" id="y" placeholder="2,4,6,8">
  <button type="submit">Calculate</button>
</form>

<script>
document.getElementById("regression-form").addEventListener("submit", async (e) => {
  e.preventDefault();
  const x = document.getElementById("x").value.split(",").map(Number);
  const y = document.getElementById("y").value.split(",").map(Number);
  const response = await fetch("/api/regression", {
    method: "POST",
    headers: {"Content-Type": "application/json"},
    body: JSON.stringify({x, y})
  });
  const result = await response.json();
  alert(`Slope: ${result.slope}, Intercept: ${result.intercept}`);
});
</script>
```



## ▶️ Cómo ejecutar la aplicación

1. **Abrir el proyecto en IntelliJ IDEA**

   * En la pantalla principal, haz clic en **Open**
   * Selecciona la carpeta `lineal-regression-app` (donde está `pom.xml`)

2. **Configurar el JDK (si es necesario)**

   * Si aparece el mensaje *“No SDK configured”*, selecciona **JDK 21 (Microsoft OpenJDK)**.
   * IntelliJ descargará y configurará automáticamente.

3. **Ejecutar la aplicación**

   * Abre el archivo `LinealRegressionAppApplication.kt`
   * Clic derecho → **Run 'LinealRegressionAppApplication'**

4. **Verificar en la consola**
   Debes ver algo como:

   ```
   Tomcat started on port(s): 8080 (http)
   Started LinealRegressionAppApplication in 2.1 seconds
   ```

5. **Abrir en el navegador**
   👉 [http://localhost:8080](http://localhost:8080)

6. **Probar la app**

   * Ingresa valores de X y Y separados por comas.
   * Haz clic en “Calculate”.
   * Verás un mensaje con la pendiente e intercepto.



## 🧩 Ejemplo de uso

| X | Y |
| - | - |
| 1 | 2 |
| 2 | 4 |
| 3 | 6 |
| 4 | 8 |

🔹 Resultado:

```
Slope: 2.0
Intercept: 0.0
```



## 🛠️ Tecnologías utilizadas

* **Kotlin** 2.0
* **Spring Boot** 3.5.7
* **Maven**
* **HTML + JavaScript**
* **Tomcat Embedded Server**



## ✨ Autora

Proyecto desarrollado por **Ana Maria Hernandez Zea y Johan Steven Galeano Gonzalez** 
Facilita la comprensión de cómo se implementa una regresión lineal simple y cómo integrarla en una aplicación web moderna con Kotlin.


