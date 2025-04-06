# Práctica 1: Creación de estructura de carpetas para un proyecto Angular con comandos Linux

## 1. Título

Estructura inicial de un proyecto Angular sin utilizar Angular CLI mediante comandos nativos de Linux.

## 2. Tiempo de duración

Aproximadamente 10 minutos en total para desarrollar toda la práctica.

## 3. Fundamentos

Angular es un framework de desarrollo frontend moderno y muy utilizado para crear aplicaciones web. Normalmente, cuando iniciamos un proyecto en Angular se hace a través del comando `ng new`, el cual genera automáticamente toda la estructura. Pero en esta práctica es replicar la estructura de forma manual utilizando únicamente comandos básicos de Linux, sin utilizar Angular CLI.

Esto nos permite comprender cómo se crearan con comandos Linux archivos y carpetas en un proyecto, lo cual es importante para un buen manejo del proyecto mediante la terminal. Además, fortalece el uso de comandos como `mkdir`, `touch`, y `tree`, que son herramientas muy importantes para la automatización de tareas.

La carpeta `src/` contiene el código fuente del proyecto, donde encontramos `app/` con sus componentes y módulos, `assets/` para los recursos estáticos como imágenes, fuentes y estilos, y otros archivos como `index.html`, `main.ts`, `styles.css` y `favicon.ico`.

En la raíz del proyecto se encuentran archivos importantes como:

- `README.md`: documentación general del proyecto.
- `angular.json`: configuración del proyecto Angular.
- `package.json`: información del proyecto y sus dependencias.
- `tsconfig.json`: configuración del compilador TypeScript.


## 4. Conocimientos previos

Para realizar esta práctica fue necesario tener claros los siguientes temas:

- Comandos básicos de Linux (`mkdir`, `touch`, `cd`, `tree`).
- Organización de archivos en proyectos Angular.
- Conceptos básicos de frontend como HTML, CSS y TypeScript.
- Navegación por la terminal.

## 5. Objetivos a alcanzar

- Comprender y reproducir manualmente la estructura básica de un proyecto Angular.
- Utilizar correctamente comandos nativos de Linux para crear carpetas y archivos.

## 6. Equipo necesario

- Computador con sistema operativo macOS o Linux (en mi caso macOs).
- Terminal de comandos Zsh o Bash.

## 7. Material de apoyo

- Manual de comandos de Linux.
- Guía de estructura de proyectos Angular.

## 8. Procedimiento

### Paso 1: Crear la carpeta principal del proyecto

```bash
cd Desktop/
mkdir -p proyecto-angular-damian/{src/app/{components,assets/{images,fonts},styles},dist,public}
```

Creo de forma recursiva todas las carpetas necesarias.

### Paso 2: Ingresar al proyecto

```bash
cd proyecto-angular-damian/
```

### Paso 3: Crear los archivos base en la raíz

```bash
touch {README.md,angular.json,package.json,tsconfig.json}
```

### Paso 4: Crear los archivos en la carpeta `src`

```bash
touch src/{index.html,main.ts,styles.css,favicon.ico}
```

### Paso 5: Crear los archivos en `src/app`

```bash
touch src/app/{app.component.ts,app.component.html,app.component.css,app.module.ts}
```

### Paso 6: Verificar la estructura usando el comando `tree`

```bash
tree
```

**Figura 1.1. Comandos ejecutados paso a paso en terminal**  
<p align="center">
  <img src="img/Captura de Pantalla 2025-04-06 a la(s) 2.41.24 a.m..png" alt="" width="800px">
</p>


## 9. Resultados esperados

Al finalizar la práctica, se obtuvo una estructura limpia y organizada del proyecto Angular, que se visualizó correctamente con el comando `tree`. La estructura contiene todas las carpetas y archivos del proyecto.Se cumplio el objetivo principal el cual era organizar el proyecto correctamente usando únicamente comandos.

**Figura 1.2. Resultado final de la estructura de carpetas del proyecto Angular**  
<p align="center">
  <img src="img/Captura de Pantalla 2025-04-06 a la(s) 2.39.18 a.m..png" alt="" width="800px">
</p>


## 10. Bibliografía

Marco. (n.d.). *Manejo de archivos y directorios a través de comandos*. UTN Facultad Regional Córdoba. https://www.investigacion.frc.utn.edu.ar/labsis/publicaciones/apunte_linux/mmad.html
