---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Desarrollo de microservicios con Typescript y NestJS
info: |
  ## Taller de TypeScript y NestJS
  ### Instructor Jassyr Juárez Vázquez

# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# take snapshot for each slide in the overview
overviewSnapshots: true
---

# Desarrollo de microservicios con Typescript y NestJS

Jassyr Juárez Vázquez

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Presiona espacio para avanzar <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <!-- <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button> -->
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
transition: fade-out
---

# Bienvenida y presentación
- **Instructor**: Jassyr Juárez Vázquez
- **Objetivos**:
  - Conocer TypeScript y sus ventajas
  - Obtener una visión general de NestJS
  - Desarrollar una aplicación sencilla con NestJS y TypeScript

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);  
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# Agenda
1. Introducción a TypeScript
2. Introducción a NestJS
3. Profundizando en NestJS y TypeScript
4. Mejores Prácticas y Técnicas Avanzadas
5. Q&A y Cierre

---

# ¿Qué es TypeScript?

TypeScript es un lenguaje de código abierto desarrollado por Microsoft. Se trata de un supraconjunto de JavaScript, lo que significa que al saber Javascript se puede extender su conocimiento al incluir características que antes no estaban disponibles.

## Tipado estático opcional

La característica principal de TypeScript es su sistema de tipos. Se puede identificar el tipo de datos de una variable o un parámetro mediante una sugerencia de tipo. Con las sugerencias de tipo, se describe la forma de un objeto, lo que proporciona una documentación mejor y permite a TypeScript validar que el código funciona correctamente.

## Herramientas de desarrollo mejoradas

Mediante la comprobación de tipos estáticos, TypeScript al principio del desarrollo detecta problemas de código que JavaScript normalmente no puede detectar hasta que el código se ejecuta en el explorador. Los tipos también permiten describir lo que debe hacer el código.

---

# Prerequisitos

Node debe estar instalado. 

Verificar la versión con `node --version`. Si no está instalado, ejecutar:
```shell
nvm install 18.20.4
nvm use 18.20.4
```
Instalar  typescript
```shell
npm install -g typescript
```
Crear una carpeta para trabajar en los ejemplos e inicializar typescript en ella
```bash
mkdir proyecto-typescript
cd proyecto-typescript
tsc --init
```

Otra opción en línea es [Typescript playground](https://www.typescriptlang.org/es/play/)

---

## Ejercicio práctico
Crear archivos con código Typescript, compilarlos con tsc y ejecutarlos con node

```ts {all}{lines:true} twoslash
// declaración de variables con tipos predefinidos
let nombre: string = 'Restaurante El Buen Sabor';
let capacidad: number = 50;
let abierto: boolean = true;

console.log(`Nombre: ${nombre}, Capacidad: ${capacidad}, Abierto: ${abierto}`);

// Inferencia de tipos
let ciudad = "Apizaco"
const poblacion = 100000
```

```bash
tsc ejemplo.ts
node ejemplo.js
```

Salida:

```shell
Nombre: Restaurante El Buen Sabor, Capacidad: 50, Abierto: true
```

---

# Tipos de datos

## boolean
```ts {all}{lines:true} twoslash
let flag: boolean;
let yes = true;
let no = false;
```

## number
```ts {all}{lines:true} twoslash
let x: number;
let y = 0;
let z: number = 123.456;
let big: bigint = 100n;
```

## string
```ts {all}{lines:true} twoslash
let nombre: string = "Jassyr";
let presentacion: string = `Mi nombre es ${nombre}.
    Hola TypeScript!.`;
console.log(presentacion);
```
---

# Tipos de datos

## any

```ts {all}{lines:true} twoslash
let randomValue: any = 10;
randomValue = 'Mateo';   // OK
randomValue = true;      // OK
```

## unknown
```ts {all}{lines:true} twoslash
let randomValue: unknown = 10;
randomValue = true;
randomValue = 'Mateo';

if (typeof randomValue === "string") {
  console.log((randomValue as string).toUpperCase());    // MATEO 
} else {
  console.log("No es un string");    // Error
}
```

---

# Tipos de datos
## array

```ts {all}{lines:true} twoslash
let lista: number[] = [1, 2, 3];
let arreglo: Array<number> = [1, 2, 3];
```

## tuplas

```ts {all}{lines:true} twoslash
let persona1: [string, number] = ['Maria', 35];
let persona2: [string, number] = ['Jose', 35, false];
```

---

# Interfaces

```ts {all}{lines:true} twoslash
interface IEmpleado {
    nombre: string;
    apellido?: string; // opcional
    nombreCompleto(): string;
    
}

let empleado: IEmpleado = {
    nombre : "Juan",
    apellido: "Sánchez",
    nombreCompleto: (): string => {
        return `${empleado.nombre} ${empleado.apellido}`;
    }    
}

empleado.edad = 10;  
```

---

# Tipos

```ts {all}{lines:true} twoslash
type Empleado = {
  nombre: string;
    apellido: string;
    nombreCompleto(): string;    
}

let empleado: Empleado = {
  nombre : "Juan",
    apellido: "Sánchez",
    nombreCompleto(){  return `${empleado.nombre} ${empleado.apellido}`; }
}
```

La principal diferencia es que un alias de tipos no se puede volver a abrir para agregar nuevas propiedades, mientras que una interfaz siempre es extensible.

---

# Funciones

TypeScript simplifica el desarrollo de funciones y facilita la solución de problemas, ya que permite escribir parámetros y valores devueltos. TypeScript también agrega nuevas opciones para los parámetros. Por ejemplo, aunque todos los parámetros son opcionales en las funciones de JavaScript, puede optar por hacer que los parámetros sean necesarios u opcionales en TypeScript.

```ts {all}{lines:true} twoslash
function sumar (x: number, y: number): number {
  return x + y;
}
console.log(sumar(1, 2)); // 3
```

Una expresión de función (o función anónima) es una función que no está cargada previamente en el contexto de ejecución y solo se ejecuta cuando el código la encuentra.

```ts {all}{lines:true} twoslash
let suma = function (x: number, y: number): number {
  return x + y;
}
suma(1, 2);
```

---

# Funciones flecha (Arrow functions 🏹)

Las funciones de flecha (también denominadas "expresión lambda" o "funciones de flecha Fat" debido al operador => usado para definirlas) proporcionan una sintaxis abreviada para definir una función anónima.


<div grid="~ cols-2 gap-2" m="t-2">

```ts {all}{lines:true} twoslash
// Función anónima
let suma1 = function (x: number, y: number) {
  return x + y;
}
```

```ts {all}{lines:true} twoslash
// Arrow function
let suma2 = (x: number, y: number) => x + y;
```


```ts {all}{lines:true} twoslash
let suma = function (input: number[]): number {
  let total = 0;  
  for (const n of input) { 
    total += n 
  }
  return total;
} 
```

```ts {all}{lines:true} twoslash
let suma2 = (input: number[]) => 
   input.reduce((a,b) => a+b, 0);
```
</div>

En algunos casos se puede reducir el cuerpo de la función a una sóla línea. Cuando esto ocurre se puede omitir el `return` y las llaves

---

# Parametros opcionales

Typescript soporta parametros opciones con el signo de interrogación (`?`) al final del nombre del parámetro. Los parametros opcionales deben ir después de los obligatorios.

```ts {all}{lines:true} twoslash
function sumar (x: number, y?: number): number {
  if (y === undefined) {
    return x;
    } else {
      return x + y;
    }
}

console.log(sumar(1, 2)); // 3
console.log(sumar(1));    // 1
```
---

# Clases

Las clases de TypeScript amplían la funcionalidad de ES6 agregando características específicas de TypeScript como anotaciones de tipo para los miembros de clase, modificadores de acceso y la capacidad de especificar parámetros obligatorios u opcionales.

Tal como en otros lenguajes OO, las `clases` en Typescript definen datos y comportamientos de lo que serán los `objetos` 


```ts {all}{lines:true} twoslash
class Persona {
    nombre: string;
    edad: number;    

    constructor(nombre: string, edad: number) {
        this.nombre = nombre;
        this.edad = edad;        
    }

    saludar() {
        console.log(`Hola, me llamo ${this.nombre} y tengo ${this.edad} años.`);
    }
}
```
---

# Componentes de clase


```ts {all}{lines:true} twoslash
class Persona {
  private _nombre: string; // propiedades o campos, son los datos del objeto
  private edad: number; 

  constructor(nombre: string, edad: number) { // función especial para crear e inizializa el objeto
      this._nombre = nombre;
      this.edad = edad;        
  }

  saludar() { // metodos de la clase son funciones que definen el comportamiento del objeto
    return `Hola, me llamo ${this.nombre} y tengo ${this.edad} años.`; 
  } 

  get nombre() { return this._nombre } // descriptor de acceso get o set
  set nombre(nombre: string) { this._nombre = nombre }
}
```

Dependiendo del caso de uso, se podrían usar algunas de las características antes mencionadas. **No** todos los elementos son necesarios.

---

# Constructor
```ts {all}{lines:true} twoslash
class Vehiculo {
  marca: string;

  modelo: string;

  // sólo un constructor por clase
  constructor(marca: string) {
    this.marca = marca;    
  }
}

```

Como las funciones, los constructores aceptan una lista de parametros que pueden ser obligatorio, opcionales o deconstrucciones. Para indicar un miembro de la clase se utiliza la palabra clave `this`.

---

# Método de clase

Se puede definir cualquier función de TypeScript dentro de una clase y llamarla como un método en el objeto o desde otras funciones dentro de la clase. Estos miembros de la clase describen los comportamientos que las clases pueden realizar.

```ts {all}{lines:true} twoslash

class Vehiculo {
  modelo: string;
  constructor(modelo: string) {
    this.modelo = modelo;
  }

  // ---cut-before---
  acelerar(velocidad: number): string {
      return `${this.ref()} esta acelerando a ${velocidad} kph.`
  }
  frenar(): string {
      return `${this.ref()} esta frenando.`
  }
  girar(direccion: 'izquierda' | 'derecha'): string {
      return `${this.ref()} gira a la ${direccion}`;
  }

  ref(): string {
      return this.modelo;
  }
// ---cut-after---
}
```
Los métodos no necesitan la palabra clave `function` cuando se declaran dentro de una clase.

---

# Creación de instancias de una clase

Para crear una instancia, se usa la palabra clave `new`

```ts {monaco-run} {autorun:false}

class Vehiculo {
  marca: string;
  _modelo: string;
  constructor(marca: string, modelo: string) {
    this.marca = marca;
    this._modelo = modelo;
  }

// demás métodos
  get modelo() { return `El modelo es ${this._modelo}`}
}

let auto = new Vehiculo('Mazda', 'CX30');

console.log(auto._modelo); 
console.log(auto.modelo);
```
<div v-click>
Por default las propiedades son de acceso público. También se pueden declarar como <code>private</code> o <code>protected</code>
</div>

---
src: ./pages/nestjs.md
---


---


# Q&A y Cierre
- Preguntas y respuestas
- Recursos adicionales:
  - Repositorios de ejemplo en GitHub
  - Documentación oficial y tutoriales

---

# ¡Gracias por asistir!
- Instructor: Jassyr Juárez Vázquez
- Contacto: linkedin.com/in/jassyrj


<PoweredBySlidev mt-10 />
