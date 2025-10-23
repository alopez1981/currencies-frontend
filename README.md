# Currencies Frontend

Este proyecto es el frontend de una aplicación de gestión y conversión de divisas, construido con **Vue 3** y que se
conecta a una API de Laravel personalizada. Permite visualizar las tasas de cambio de diferentes divisas (respecto al
USD) y realizar conversiones entre ellas.

## Características

* **Listado de Divisas:** Muestra un listado actualizado de las divisas disponibles, incluyendo su código, nombre y la
  tasa de cambio respecto al Dólar Estadounidense (USD).
* **Conversión de Tasas:** Permite a los usuarios seleccionar dos divisas y obtener el valor de una respecto a la otra (
  ej. EUR respecto a USD).
* **Actualizaciones Horarias:** La API backend se encarga de obtener y actualizar las tasas de cambio cada hora
  desde [Fixer.io](https://fixer.io/), asegurando que la información mostrada esté siempre al día.
* **Desarrollado con Vue 3:** Utiliza la Composition API de Vue 3 para una lógica de componentes reactiva y modular.
* **Conexión con API REST:** Se comunica con el backend de Laravel a través de `axios` para consumir los datos de
  divisas y conversiones.
* **Estilo CSS:** Interfaz de usuario estilizada con CSS puro.

## Tecnologías Utilizadas

* **Frontend:**
    * Vue 3 (Composition API)
    * Vite (como herramienta de compilación)
    * Axios (para peticiones HTTP)
    * CSS (estilado)
* **Backend (Conexión con):**
    * Laravel (PHP)
    * Fixer.io API

## Configuración del Proyecto

### Prerrequisitos

Antes de ejecutar este frontend, asegúrate de tener:

1. **Node.js y npm (o Yarn)** instalados en tu sistema.
2. **La API de Laravel en funcionamiento:** Este frontend depende de la API backend que has desarrollado. Asegúrate de
   que esté desplegada y accesible. La API debe exponer los siguientes endpoints:

* `GET /api/currencies`: Para obtener el listado de divisas con `code`, `name` y `rate_USD`.
* `GET /api/rate-conversion?from={currency_code}&to={currency_code}`: Para obtener la tasa de conversión entre dos
  divisas.

### Instalación

1. **Clona este repositorio:**

   ```bash
   git clone [https://github.com/alopez1981/currencies-frontend.git](https://github.com/alopez1981/currencies-frontend.git)
   cd currencies-frontend
   ```

2. **Instala las dependencias de Node.js:**

   ```bash
   npm install
   # o
   yarn install
   ```

3. **Configura las variables de entorno:**
   Crea un archivo `.env` en la raíz del proyecto (si no existe) y define la URL base de tu API de Laravel.

   ```dotenv
   VITE_APP_API_URL=http://localhost:8000/api # Ajusta esto a la URL de tu API
   ```

   *Asegúrate de que la URL termine en `/api` si tus rutas de API en Laravel usan ese prefijo.*

## Ejecución del Proyecto

1. **Inicia el servidor de desarrollo:**

   ```bash
   npm run dev
   # o
   yarn dev
   ```

   Esto iniciará el frontend en modo de desarrollo, usualmente en `http://localhost:5173`. Abre esta URL en tu
   navegador.

2. **Crea una versión para producción:**

   ```bash
   npm run build
   # o
   yarn build
   ```

   Este comando generará los archivos estáticos de tu aplicación en la carpeta `dist/`, listos para ser desplegados en
   un servidor web estático.

## Estructura del Proyecto

La estructura del proyecto sigue las convenciones de Vue CLI con algunas adaptaciones para la Composition API:

