# NestJs

---
layout: default
---

# ¿Qué es NestJs?

Un framework para la construcción de aplicaciones Node.js del lado del servidor. Contiene una serie de características que lo colocan como una referencia en la industria para desarrollar APIs y microservicios. Soporta TypeScript y sus arquitectura esta inspirada en Angular.

Facilita el desarrollo de aplicaiones escalables, fáciles de probar y mantener. 

---

# Instalación de NestJs y aplicaciones requeridas


## NestJS

```shell
npm i -g @nestjs/cli
```

## DynamoDB

```shell
docker run -d -p 8000:8000 amazon/dynamodb-local
npm install -g dynamodb-admin
```

Docker debe estar corriendo en la maquina local. 

Posteriormente, en una nueva ventana, iniciar `dynamodb-admin`
La interfaz de Dynamo estará disponible en la dirección: `http://localhost:8001/`

---

# Creación de un nuevo proyecto con NestJS
```shell
nest new seminario
> npm
....
cd seminario
npm run start
```

Ahora en postman, crear una nueva coleccion y hacer una petición GET a `http://localhost:3000/`

---

# Desarrollo de una API REST básica
## Crear un CRUD
 
 El CLI de NestJS ofrece la posibilidad de generar el esquelo de un CRUD mediante el siguiente comando:

 ```shell
 nest g resource taller
 ```

Esto creará los siguientes elementos:

- Un módulo: `taller.module.ts`
- Un controlador: `taller.controller.ts` configurado para la ruta /taller
- Un servicio: `taller.service.ts` donde esta la logica de negocio
- Entidades y dtos: Para la transferencia de datos en las capas de la aplicación

--- 

# Controladores

Los controladores son responsables por manejar las solicitudes y respuestas a los clientes.

Un controlador es una clase anotada con `@Controller()`. La anotación `@Controller` puede recibir un parametro string que definirá su ruta

```ts
import { Controller, Get, Post, Body, Patch, Param, Delete } from '@nestjs/common';

@Controller('taller')
export class TallerController {
  constructor(private readonly tallerService: TallerService) {}

  @Post()
  create(@Body() createTallerDto: CreateTallerDto) {
    return this.tallerService.create(createTallerDto);
  }
  
  // Otros métodos del controlador
}
```
El mecanismo de ruteo de NesJS determina que controlador recibirá las peticiones. Un controlador puede tener mas de una ruta, y cada ruta puede realizar diferentes acciones.

---

# Servicios

Los servicios contienen la lógica de negocio. Son los elementos reusables dentro de las aplicaciones de NestJS

```ts
import { Injectable } from '@nestjs/common';
import { CreateTallerDto } from './dto/create-taller.dto';

@Injectable()
export class TallerService {
  create(createTallerDto: CreateTallerDto) {
    return 'This action adds a new taller';
  }

  findAll() {
    return `This action returns all taller`;
  }
  /// otros métodos del servicio
}
```

---

# Modulos

Agrupan los componentes de manera lógica (por dominio). Un módulo puede contener uno o varios controladores y servicios. Los módulos pueden exportarse e importarse en otros módulos.

```ts
import { Module } from '@nestjs/common';
import { TallerService } from './taller.service';
import { TallerController } from './taller.controller';

@Module({
  controllers: [TallerController],
  providers: [TallerService],
})
export class TallerModule {}
```

Todos los archivos se crean dentro de la carpeta taller. De esta manera NestJS separa los conceptos de dominio en módulos.

---

# Repositorios para acceder a la base de datos DynamoDB

## Crear un módulo donde se ubique el código para acceder a DynamoDB

```shell
npm install @aws-sdk/lib-dynamodb
npm install @aws-sdk/client-dynamodb
npm install uuid
npm install @types/uuid --save-dev

nest g module database
cd .\src\database\ 
nest g service DynamoDB --flat
nest g service TallerRepository --flat
```

---

# Definición de entidades

## Definir las propiedades de la clase Taller y sus DTOs

```ts 
export class Taller {
  pk: string;
  titulo: string;
  fecha: string;
  creditos: number;
}

export class CreateTallerDto {
  titulo: string;
  fecha: string;
  creditos: number;
}
```
---

# Configurar el proyecto para usar DynamoDB

1. Actualizar el módulo DataBase

```
@Module({
  providers: [
    DynamoDbService,
    TallerRepositoryService,
    {
      provide: DynamoDBDocumentClient,
      useFactory: () => {
        const client = new DynamoDBClient({
          region: 'us-east-2',
          endpoint: 'http://localhost:8000',
        });
        return DynamoDBDocumentClient.from(client);
      },
    },
  ],
  exports: [TallerRepositoryService],
})
export class DatabaseModule {}
```


---

2. Implementar un servicio para comunicarse con DynamoDB



Descargar el archivo base [DynamoDBService](https://drive.google.com/drive/folders/1VVkYY3EKckZoP9WINlYqEFQq6-asVNbr?usp=sharing)

Implementar los métodos de la clase `TallerRepository`

- `create(createTallerDto: CreateTallerDto): Promise<Taller>`
- `findAll(): Promise<Taller[]>` 
- `findById(id: string): Promise<Taller>`
- `update(id: string, updateTallerDto: UpdateTallerDto)`
- `delete(id: string)` 


--- 


# Validación y manejo de errores
Uso de Pipes

Usando `class-validator`. NestJS ya proporciona mecanismos de validación. Ver 
[Validación](docs.nestjs.com/techniques/validation)

```shell
npm install class-validator --save
npm install class-transformer --save
``` 

```ts
export class CreateTallerDto {
  @IsString()
  titulo: string;
  @IsDateString()
  fecha: string;
  @IsPositive()
  creditos: number;
}
```

El controlador debe especificar el `pipe` de validación 
```ts 
@UsePipes(new ValidationPipe({ transform: true }))
```

--- 

# Extra

Implementar un CRUD para el registro de asistentes a un taller

- Crear el controller, servicio y entidades con `nest g resource asistente`
- Validar los datos con `class-validator`
  - `@IsEmail()`  
- Actualizar el taller 
  - Agregar cupo máximo
  - Agregar cupo actual
  - Devolver 404 cuando el cupo se ha llenado

# Material adicional a investigar por cuenta propia

- Jest para pruebas unitarias (El cli de NestJS ya crea las plantillas de las pruebas)
- PinoLogger para no usar console.log y disponer de un servicio dedicado, compatible con servicios de observabilidad como Kibana
- Authenticación/Authorizacion con `Guards`
- Manejador de errores globales con filtros
- Despliegue en la nube en railway, aws, vercel y otros.
- Integración con BaaS
- Redis para emisión y consumo de eventos
- Cache para performance
