# Chat ordenado por flujo de app (preguntas y respuestas)

> Nota: aquí está todo ordenado por flujo de la app.  
> Se mantienen las explicaciones tal como se dieron en el chat.

---

## 1) Qué es Formly y cómo funciona con tu JSON/API

### Pregunta
`que es y como funciona formly explicamelo ... explicame como es el json que uso en api y como lo uso y muestro lo ue pongo`

### Respuesta
`Formly` es una librería para Angular que te permite **definir formularios con JSON** en vez de escribir todo el HTML a mano.

En tu proyecto, la idea es:

- El backend (o mock API) devuelve un JSON con `fields`.
- Angular lo carga con `HttpClient`.
- `<formly-form>` dibuja automáticamente los campos.
- El usuario completa datos.
- Al enviar, conviertes `model` a payload final y lo usas (ahora lo estás mostrando por consola).

## Cómo funciona en tu app

1. El servicio `FormConfigService` pide este archivo:
   - `src/api/public/mocks/v1/parameters/service-contract-form.json`
2. En `ServiceContractFormComponent`, guardas:
   - `this.fields = config.fields`
   - `this.submitLabel = config.submitLabel`
3. En el template, `formly-form` renderiza todo con:
   - `[fields]="fields"`
   - `[model]="model"`
   - `[form]="form"`

O sea: **el JSON manda** qué campos aparecen y con qué reglas.

## Cómo es el JSON que usas en API

Tu JSON tiene esta forma general:

- `submitLabel`: texto del botón submit.
- `fields`: array de campos Formly.

Cada campo normalmente tiene:

- `key`: nombre de la propiedad en el `model` (`name`, `email`, `service`, etc.).
- `type`: tipo visual (`input`, `radio`, o tu custom `service-selector`).
- `props`: label, placeholder, required, options, min, etc.
- `validation.messages`: mensajes de error.
- `expressions` (opcional): lógica dinámica para mostrar/ocultar y requerir.
- `resetOnHide` (opcional): limpia valor si se oculta.

Ejemplo conceptual (como en tu caso):
- Si `model.service !== 'electricity'`, ocultas `rateElectricity` y `power`.
- Si `model.service === 'gas'`, muestras `rateGas` y `consumption`.

## Cómo “mostrar lo que pongo” (lo que escribe el usuario)

En Formly, todo se guarda en `model`. En tu componente ya sucede.

Ahora mismo lo muestras en submit con `console.log(...)` en `submitForm()` mediante `_buildSubmitPayload()`.

Ejemplo de salida final si elige gas:

```json
{
  "name": "Juan",
  "email": "juan@mail.com",
  "service": "gas",
  "rateGas": "tur",
  "consumption": 120
}
```

Si quieres verlo en pantalla en tiempo real (además de consola), puedes poner en el HTML:

```html
<pre>{{ model | json }}</pre>
```

Eso te muestra exactamente lo que el usuario va cargando en el formulario.

---

Si quieres, en el siguiente paso te explico **campo por campo de tu JSON actual** y te digo qué parte se renderiza en pantalla para cada uno (`name`, `email`, `service`, `rateElectricity`, `power`, `rateGas`, `consumption`).

---

## 2) Formly personalizado (custom type)

### Pregunta
`ahora explicame que es un formly hecho personalisado como funciona`

### Respuesta
Un **Formly personalizado** (custom type) es cuando creas **tu propio tipo de campo** para usarlo dentro del JSON, en lugar de usar solo los tipos nativos (`input`, `select`, `radio`, etc.).

En tu caso, el personalizado es: `service-selector`.

## Qué significa “personalizado”

Normalmente en Formly pones:

- `type: "input"` o `type: "radio"`.

Pero tú creaste uno nuevo:

- `type: "service-selector"`.

Eso le dice a Formly:
“Para este campo, no uses un input normal. Usa **mi componente Angular**”.

---

## Cómo funciona internamente (en tu proyecto)

1. En el JSON defines:
   - `key: "service"`
   - `type: "service-selector"`
   - `props.options` con `gas` y `electricity`.

2. En `FormlyTypesModule` registras ese tipo:
   - nombre: `service-selector`
   - componente: `ServiceSelectorFieldComponent`

3. Cuando Formly lee ese `type`, renderiza `ServiceSelectorFieldComponent`.

4. Ese componente hereda de `FieldType`, así que ya recibe:
   - `formControl` (control real de Angular)
   - `props` (label, options, required, etc.)
   - estado de errores (`showError`, touched, invalid)

5. Cuando haces click en una tarjeta:
   - ejecuta `selectService(value)`
   - actualiza `formControl.setValue(value)`

6. Al cambiar `service`, Formly recalcula expresiones de otros campos:
   - mostrar/ocultar (`hide`)
   - requerido dinámico (`props.required`)

Por eso se activan campos de gas o electricidad automáticamente.

---

## Idea clave para que lo expliques fácil

Tu campo personalizado **solo cambia la forma visual y UX** (tarjetas clicables), pero sigue siendo un **control reactivo real** dentro del mismo formulario.  
No rompe Formly: se integra al flujo normal de validación, modelo, errores y submit.

---

Si quieres, te hago una explicación “para exponer en 20 segundos” y otra “técnica para profe” (más profunda).

---

## 3) Constructor, DI e `inject()`

### Pregunta
`@src/app/features/service-contract/components/service-contract-form/service-contract-form.component.ts:28 eso se pone asi contructor??`

### Respuesta
Sí, está **bien escrito** así:

`constructor(private readonly _formConfigService: FormConfigService) {}`

Solo ojo con el nombre: se escribe **`constructor`**, no “contructor”.

## Qué hace esa línea

- `constructor(...)`: es el constructor de la clase en TypeScript/Angular.
- `private`: la propiedad solo se usa dentro de la clase.
- `readonly`: no la puedes reasignar después.
- `_formConfigService: FormConfigService`: Angular inyecta automáticamente ese servicio (DI).

Es una forma corta equivalente a:

```ts
private readonly _formConfigService: FormConfigService;

constructor(formConfigService: FormConfigService) {
  this._formConfigService = formConfigService;
}
```

Así que sí: en tu componente está correcto.

---

### Pregunta
`pero eso no seria lo que hace una interfas sin necesidad de escribir constructor?`

### Respuesta
Buena duda: **no**, una `interface` no reemplaza al `constructor`.

- Una `interface` en TypeScript **solo define forma/tipo** (qué propiedades o métodos debe tener algo).
- El `constructor` **crea/inicializa** la instancia de la clase y, en Angular, además sirve para **inyección de dependencias** (`DI`).

En tu caso:

- `FormConfigService` es una clase/servicio real que Angular tiene que inyectar.
- Por eso se pone en el `constructor(...)`.
- La interface no “inyecta” nada ni crea objetos.

Ejemplo rápido:

- `interface User { name: string }` -> solo tipo.
- `constructor(private userService: UserService) {}` -> Angular te entrega una instancia real de `UserService`.

Si quieres, te muestro cuándo sí usar `interface` en tu proyecto (por ejemplo en `ServiceFormConfig`) y cuándo sí o sí `constructor` (servicios como `FormConfigService`).

---

### Pregunta
`cual seria la manera de hacer lo mismo que  aqui constructor? @src/app/features/service-contract/components/service-contract-form/service-contract-form.component.ts:27-28`

### Respuesta
La forma “equivalente” (sin usar la versión corta en parámetros del constructor) sería esta:

```ts
export class ServiceContractFormComponent implements OnInit, OnDestroy {
  private readonly _formConfigService: FormConfigService;

  constructor(formConfigService: FormConfigService) {
    this._formConfigService = formConfigService;
  }

  // ...resto del componente
}
```

Hace exactamente lo mismo que esto:

```ts
constructor(private readonly _formConfigService: FormConfigService) {}
```

## Resumen rápido

- **Tu forma actual** = sintaxis corta (más usada en Angular).
- **La alternativa** = sintaxis explícita (declaras propiedad + asignas dentro del constructor).
- Con `inject()` (Angular moderno) también se puede sin constructor, pero es otro estilo.

Si quieres, te enseño también la versión con `inject(FormConfigService)` para que compares las 3.

---

### Pregunta
`digo sin usarlo`

### Respuesta
Sí, se puede hacer **sin `constructor`** usando `inject()` de Angular.

En tu componente sería así:

```ts
import { Component, OnDestroy, OnInit, inject } from '@angular/core';
import { FormConfigService } from '../../../../core/services/form-config.service';

export class ServiceContractFormComponent implements OnInit, OnDestroy {
  private readonly _formConfigService = inject(FormConfigService);

  // sin constructor
}
```

Con eso logras lo mismo: Angular te inyecta `FormConfigService`, pero usando una propiedad de clase en vez del constructor.

- `constructor(...)` = forma clásica.
- `inject(...)` = forma moderna, sin constructor.

Si quieres, te lo adapto completo en tu archivo para que quede ya funcionando así.

---

### Pregunta
`bueno ahora explicame como funciona el contructor y como funciona el injert`

### Respuesta
Perfecto. Te explico ambos de forma simple:

## `constructor` (inyección clásica)

En Angular, el `constructor` se ejecuta cuando se crea el componente.  
Si pones un servicio en sus parámetros, Angular lo **inyecta automáticamente**.

Ejemplo:

```ts
constructor(private readonly _formConfigService: FormConfigService) {}
```

Qué pasa aquí:

- Angular ve que necesitas `FormConfigService`.
- Busca una instancia en su sistema de DI (Dependency Injection).
- Te la entrega.
- Además, TypeScript crea la propiedad `_formConfigService` por ti (por `private readonly`).

Uso típico: estilo tradicional, muy común en Angular.

---

## `inject()` (forma moderna, sin constructor)

`inject()` hace lo mismo (pedir dependencias al sistema DI), pero desde una propiedad/campo de clase.

Ejemplo:

```ts
private readonly _formConfigService = inject(FormConfigService);
```

Qué pasa aquí:

- Cuando Angular crea el componente, esa línea se evalúa en contexto de inyección.
- Angular te da la misma instancia de `FormConfigService`.
- No necesitas declarar `constructor`.

Uso típico: componentes más limpios, menos código cuando solo inyectas dependencias.

---

## Diferencia real entre ambos

- **Los dos inyectan igual** (mismo DI de Angular).
- Cambia la **sintaxis/estilo**, no el resultado funcional.
- `constructor`: clásico y explícito.
- `inject()`: moderno y compacto.

---

## Regla práctica

- Si tu equipo usa estilo clásico → `constructor`.
- Si prefieren Angular moderno y código más corto → `inject()`.

Si quieres, te muestro tu componente exacto en ambas versiones para que veas línea por línea qué cambia y qué no.

---

### Pregunta
`explicame mas claro para que se usan`

### Respuesta
Se usan para lo mismo: **pedirle a Angular objetos que tu componente necesita** (servicios, router, http, etc.) sin crearlos a mano.

Piensa así:

- Tu componente = “empleado”.
- Angular DI = “almacén”.
- `constructor` o `inject()` = “forma de pedir herramientas al almacén”.

## Para qué se usan (en la vida real)

- Obtener un servicio para llamar API (`FormConfigService`, `HttpClient`).
- Navegar (`Router`).
- Leer ruta actual (`ActivatedRoute`).
- Mostrar notificaciones (`MessageService`, etc.).

En tu caso:
- necesitas `FormConfigService` para traer el JSON del formulario.
- lo inyectas y luego llamas `getServiceContractConfig()`.

## `constructor` vs `inject()`

- `constructor(...)`: forma clásica de pedir dependencias.
- `inject(...)`: forma nueva, sin constructor.
- **Uso práctico: igual**. Cambia solo cómo lo escribes.

## Qué NO hacen

- No reemplazan interfaces.
- No validan datos.
- No renderizan UI.
- Solo te “entregan” instancias que Angular administra.

## Frase corta para recordar

“`constructor` e `inject()` se usan para inyectar dependencias; es decir, para obtener servicios listos y usarlos en el componente sin instanciarlos manualmente.”

Si quieres, te marco en tu archivo exacto dónde se usa el servicio después de inyectarlo, para que lo veas conectado de punta a punta.

---

### Pregunta
`y como es la intancia manual`

### Respuesta
La **instancia manual** sería crear el objeto tú mismo con `new`, en vez de pedírselo a Angular DI.

## Ejemplo simple (manual, fuera de Angular DI)

```ts
class LoggerService {
  log(msg: string) {
    console.log(msg);
  }
}

const logger = new LoggerService(); // instancia manual
logger.log('hola');
```

## En tu caso con `FormConfigService`

Si intentaras manualmente:

```ts
const service = new FormConfigService(/* HttpClient */);
```

Problema:
- `FormConfigService` necesita `HttpClient` en su constructor.
- Entonces también tendrías que crear `HttpClient` manualmente (y sus dependencias internas), lo cual es complejo y no es la forma Angular.

## Por eso en Angular usamos DI

En vez de crear manualmente:

- Angular crea y gestiona instancias.
- Tú solo pides:
  - con `constructor(...)` o
  - con `inject(...)`.

Así evitas `new` y no te preocupas por dependencias internas.

## Regla práctica

- **Manual (`new`)**: útil en clases simples sin dependencias de Angular.
- **DI de Angular**: para servicios/componentes reales de app (`HttpClient`, `Router`, tus servicios `@Injectable`).

---

## 4) Componente principal del formulario (`service-contract-form.component.ts`)

### Pregunta
`@src/app/features/service-contract/components/service-contract-form/service-contract-form.component.ts:52-56 explicame este codigo`

### Respuesta
Ese bloque es una **validación de seguridad antes de enviar**.

```ts
public submitForm(): void {
  if (this.form.invalid || !this.model.service) {
    this.form.markAllAsTouched();
    return;
  }
```

Qué hace, paso a paso:

- `submitForm()` se ejecuta cuando haces clic en **Submit**.
- `if (this.form.invalid || !this.model.service)`:
  - `this.form.invalid` => el formulario tiene errores (faltan campos, email inválido, min, etc.).
  - `!this.model.service` => no se eligió servicio (`gas` o `electricity`).
  - Con `||` basta que una de las dos sea verdadera para frenar el envío.
- `this.form.markAllAsTouched()`:
  - marca todos los campos como “tocados”.
  - así Angular/Formly muestra los mensajes de error en pantalla.
- `return;`:
  - corta la función ahí.
  - no sigue con el `console.log` ni con la construcción del payload.

En resumen: **si el form no está completo/valido, no envía nada y muestra errores**.

---

### Pregunta
`@src/app/features/service-contract/components/service-contract-form/service-contract-form.component.ts:71-85 explicame este`

### Respuesta
Este bloque define **2 getters** (propiedades calculadas) para usar en el HTML de forma limpia.

## 1) `hasSelectedService`

```ts
public get hasSelectedService(): boolean {
  return !!this.model.service;
}
```

- Revisa si existe valor en `this.model.service`.
- `!!` convierte cualquier valor a booleano:
  - si hay valor (`'gas'` o `'electricity'`) -> `true`
  - si no hay valor (`undefined`, `null`, `''`) -> `false`

Para qué sirve:
- Mostrar/ocultar bloques (`*ngIf`)
- Habilitar/deshabilitar submit (`[disabled]`)

---

## 2) `selectedServiceLabel`

```ts
public get selectedServiceLabel(): string {
  if (this.model.service === 'gas') {
    return 'Gas';
  }

  if (this.model.service === 'electricity') {
    return 'Electricity';
  }

  return '';
}
```

- Convierte el valor técnico del modelo a texto amigable para UI.
- Si `service` es:
  - `'gas'` -> devuelve `'Gas'`
  - `'electricity'` -> devuelve `'Electricity'`
  - nada/otro -> devuelve `''`

Para qué sirve:
- Mostrar texto tipo: “You are contracting Gas/Electricity”.

---

En resumen:
- `hasSelectedService` = **estado booleano** (hay o no servicio elegido).
- `selectedServiceLabel` = **texto para mostrar** según servicio elegido.

---

### Pregunta
`@src/app/features/service-contract/components/service-contract-form/service-contract-form.component.ts:87-110 explicamelo detalladamente`

### Respuesta
Excelente bloque para entender. Este método es el que **transforma el modelo del formulario** en el objeto final que vas a enviar/loguear.

```ts
private _buildSubmitPayload(): ServiceFormPayload {
  const { name = '', email = '', service } = this.model;

  switch (service) {
    case 'gas':
      return {
        name,
        email,
        service,
        rateGas: this.model.rateGas as GasRateType,
        consumption: Number(this.model.consumption),
      };
    case 'electricity':
      return {
        name,
        email,
        service,
        rateElectricity: this.model.rateElectricity as ElectricityRateType,
        power: Number(this.model.power),
      };
    default:
      throw new Error('Service type is required to build payload.');
  }
}
```

## Qué hace en general

- Recibe implícitamente los datos actuales (`this.model`).
- Detecta qué servicio eligió el usuario (`gas` o `electricity`).
- Devuelve un payload diferente según el servicio.
- Garantiza un tipo de salida: `ServiceFormPayload` (unión tipada).

---

## Paso por paso

### 1) Firma del método

- `private`: solo se usa dentro del componente.
- `_buildSubmitPayload()`: nombre interno para construir payload.
- `: ServiceFormPayload`: TypeScript exige que lo que retorne sea uno de los payloads válidos (gas o electricidad).

---

### 2) Destructuring inicial

```ts
const { name = '', email = '', service } = this.model;
```

- Extrae `name`, `email`, `service` desde `this.model`.
- Si `name` o `email` vienen `undefined`, usa `''` como fallback.
- `service` se extrae tal cual porque es la clave para decidir el caso.

---

### 3) `switch (service)`

Decide la forma del objeto final:

#### Caso `'gas'`

```ts
return {
  name,
  email,
  service,
  rateGas: this.model.rateGas as GasRateType,
  consumption: Number(this.model.consumption),
};
```

- Devuelve solo los campos pertinentes a gas.
- `rateGas as GasRateType`:
  - casteo de TypeScript para decir “trátalo como tipo válido de gas”.
- `Number(this.model.consumption)`:
  - convierte el valor a número real (por si llega como string desde el form).

#### Caso `'electricity'`

```ts
return {
  name,
  email,
  service,
  rateElectricity: this.model.rateElectricity as ElectricityRateType,
  power: Number(this.model.power),
};
```

- Devuelve los campos propios de electricidad.
- Mismo patrón: casteo de tipo + conversión numérica.

---

### 4) `default` con error

```ts
throw new Error('Service type is required to build payload.');
```

- Si `service` no existe o viene inválido, el método falla de forma explícita.
- Es una defensa extra para evitar payloads inconsistentes.

---

## Por qué está bien diseñado

- Separa claramente lógica de submit vs construcción de datos.
- Mantiene payload limpio según servicio elegido.
- Evita enviar campos que no corresponden (gas vs electricidad).
- Fuerza tipado fuerte (`ServiceFormPayload`), útil para backend y mantenimiento.

---

## Nota útil

Como ya validas antes en `submitForm()` que exista `this.model.service`, el `default` casi nunca debería ejecutarse. Aun así, está bien tenerlo como “red de seguridad”.

---

## 5) Modelos TypeScript (`shared/models`)

### Pregunta
`@src/app/shared/models/form-option.model.ts:1-6explicame`

### Respuesta
Ese archivo define una **interfaz** de TypeScript llamada `FormOption`.

```ts
export interface FormOption {
  label: string;
  value: string;
  hint?: string;
}
```

## Qué significa cada campo

- `label: string`
  - Texto visible para el usuario.
  - Ej: `"Gas"`, `"Electricity"`.

- `value: string`
  - Valor interno que se guarda en el modelo.
  - Ej: `"gas"`, `"electricity"`.

- `hint?: string`
  - Texto opcional de apoyo/descripción.
  - El `?` significa que puede venir o no.
  - Ej: `"TUR and free market rates"`.

## Para qué sirve

Se usa para tipar listas de opciones (`props.options`) en tu custom field de Formly (`service-selector`), así evitas errores de estructura y tienes autocompletado.

## Ejemplo válido

```ts
const option: FormOption = {
  label: 'Gas',
  value: 'gas',
  hint: 'TUR and free market rates',
};
```

Si omites `hint`, sigue siendo válido porque es opcional.

---

### Pregunta
`explicame  @src/app/shared/models/service-form-config.model.ts:1-9`

### Respuesta
Ese archivo define el **tipo de la respuesta de configuración** que cargas desde tu JSON de API/mock.

```ts
import { FormlyFieldConfig } from "@ngx-formly/core";

export interface ServiceFormConfig {
    submitLabel: string;
    fields: FormlyFieldConfig[];
}
```

## Qué significa

- `ServiceFormConfig` = “así debe venir la config del formulario”.
- Tiene 2 propiedades obligatorias:

- `submitLabel: string`
  - Texto del botón submit.
  - Ej: `"Submit form"`.

- `fields: FormlyFieldConfig[]`
  - Arreglo de campos Formly.
  - Cada elemento describe un campo (`key`, `type`, `props`, `expressions`, etc.).

## Para qué se usa en tu app

Cuando haces:

```ts
this._http.get<ServiceFormConfig>(url)
```

le dices a TypeScript:
- “la respuesta debe tener esta estructura”.
- así tienes autocompletado y chequeo de tipos al hacer:
  - `config.submitLabel`
  - `config.fields`

## Resumen corto

Este archivo es el **contrato tipado** entre tu JSON de configuración y el componente que renderiza el formulario con Formly.

---

### Pregunta
`explicame @service-form.model.ts (1-36)  a detalle`

### Respuesta
Perfecto. Este archivo define **todos los tipos del dominio** de tu formulario: qué valores son válidos, cómo se ve el borrador (`draft`) y cómo debe quedar el payload final al enviar.

## 1) Tipos base (líneas 1-3)

```ts
export type ServiceType = 'gas' | 'electricity';
export type GasRateType = 'tur' | 'freeMarket';
export type ElectricityRateType = 'fixed' | 'variable';
```

Aquí usas **uniones de literales string**.  
Eso significa que no aceptas cualquier texto, solo valores concretos.

- `ServiceType`: solo `'gas'` o `'electricity'`.
- `GasRateType`: solo `'tur'` o `'freeMarket'`.
- `ElectricityRateType`: solo `'fixed'` o `'variable'`.

Ventaja: TypeScript te protege de errores como `'gass'`, `'FREE'`, etc.

---

## 2) Modelo editable del formulario (líneas 5-13)

```ts
export interface ServiceFormDraft {
  name?: string;
  email?: string;
  service?: ServiceType;
  rateGas?: GasRateType;
  rateElectricity?: ElectricityRateType;
  consumption?: number;
  power?: number;
}
```

`ServiceFormDraft` representa el **estado mientras el usuario llena el form**.

- Todo tiene `?` (opcional), porque durante la edición:
  - al inicio no hay datos,
  - algunos campos todavía no aparecen (por lógica condicional),
  - algunos se limpian con `resetOnHide`.

Este tipo encaja perfecto con Formly, porque el `model` se va completando gradualmente.

---

## 3) Base común del payload final (líneas 15-19)

```ts
interface ServiceFormBasePayload {
  name: string;
  email: string;
  service: ServiceType;
}
```

Esto define lo que **siempre** debe existir al enviar:

- `name`
- `email`
- `service`

Ojo: aquí ya **no son opcionales** (sin `?`), porque el payload final debe ser completo.

---

## 4) Payload final para gas (líneas 21-25)

```ts
export interface GasServiceFormPayload extends ServiceFormBasePayload {
  service: 'gas';
  rateGas: GasRateType;
  consumption: number;
}
```

Este tipo dice:

- hereda la base (`name`, `email`, `service`),
- pero fuerza que `service` sea exactamente `'gas'`,
- y exige campos de gas:
  - `rateGas`
  - `consumption`

---

## 5) Payload final para electricidad (líneas 27-31)

```ts
export interface ElectricityServiceFormPayload extends ServiceFormBasePayload {
  service: 'electricity';
  rateElectricity: ElectricityRateType;
  power: number;
}
```

Igual lógica que gas, pero para electricidad:

- `service` debe ser `'electricity'`,
- exige:
  - `rateElectricity`
  - `power`

---

## 6) Unión final del payload (líneas 33-35)

```ts
export type ServiceFormPayload =
  | GasServiceFormPayload
  | ElectricityServiceFormPayload;
```

Este es el tipo que usas al enviar.  
Significa: “el payload final será **o** de gas **o** de electricidad”.

Esto es súper importante porque crea una **unión discriminada** por `service`:

- Si `service === 'gas'`, TypeScript sabe que existen `rateGas` y `consumption`.
- Si `service === 'electricity'`, sabe que existen `rateElectricity` y `power`.

---

## Idea clave del archivo

Distingues muy bien dos etapas:

- **Draft (`ServiceFormDraft`)**: datos incompletos y opcionales mientras el usuario edita.
- **Payload (`ServiceFormPayload`)**: datos completos y coherentes al enviar, según el servicio elegido.

Ese diseño evita muchos bugs y te da tipado fuerte de punta a punta.

---

## 6) Campo personalizado (`service-selector-field.component.ts`)

### Pregunta
`@src/app/shared/formly-types/service-selector-field/service-selector-field.component.ts:10-35 explicame a detalle`

### Respuesta
Excelente, este bloque es el corazón de tu **campo personalizado de Formly**.

```ts
export class ServiceSelectorFieldComponent extends FieldType {
  public get serviceOptions(): FormOption[] {
    return (this.props['options'] as FormOption[]) || [];
  }

  public selectService(value: string): void {
    if (this.formControl.disabled) {
      return;
    }

    if (this.formControl.value === value) {
      return;
    }

    this.formControl.setValue(value);
    this.formControl.markAsDirty();
    this.formControl.markAsTouched();
  }

  public isSelected(value: string): boolean {
    return this.formControl.value === value;
  }

  public buildOptionId(value: string): string {
    return `serviceOption${value.charAt(0).toUpperCase()}${value.slice(1)}`;
  }
}
```

## 1) `extends FieldType` (línea 10)

Esto hace que tu componente sea un **control Formly real**.

Gracias a `FieldType`, ya tienes disponibles propiedades como:

- `this.formControl` -> control reactivo de Angular asociado al campo.
- `this.props` -> configuración que viene del JSON (`label`, `options`, `required`, etc.).
- `showError`, `form`, `field`, etc. (estado de Formly).

O sea: no es un componente suelto; está conectado al formulario.

---

## 2) Getter `serviceOptions` (líneas 11-13)

```ts
public get serviceOptions(): FormOption[] {
  return (this.props['options'] as FormOption[]) || [];
}
```

Qué hace:

- Lee `options` desde `props` (viene del JSON del campo `service`).
- Lo castea como `FormOption[]`.
- Si no existe, devuelve `[]` para evitar errores en el template.

Por qué está bien:

- Protege contra `undefined`.
- Permite hacer `*ngFor="let option of serviceOptions"` sin romper.

---

## 3) `selectService(value)` (líneas 15-27)

Método que se ejecuta al hacer click en una tarjeta.

### Paso a paso:

1. **Si está deshabilitado, no hace nada**
   ```ts
   if (this.formControl.disabled) return;
   ```
   Evita cambios cuando el campo no debería editarse.

2. **Si ya está seleccionado ese valor, no repite trabajo**
   ```ts
   if (this.formControl.value === value) return;
   ```
   Evita render/revalidaciones innecesarias.

3. **Actualiza el valor del control**
   ```ts
   this.formControl.setValue(value);
   ```
   Esto actualiza `model.service` en Formly.

4. **Marca el control como modificado y tocado**
   ```ts
   this.formControl.markAsDirty();
   this.formControl.markAsTouched();
   ```
   - `dirty`: el usuario cambió el valor.
   - `touched`: hubo interacción.
   Esto ayuda a validaciones y mensajes de error.

Impacto clave:
- Al cambiar `service`, Formly recalcula expresiones (`hide`, `props.required`) del resto de campos.

---

## 4) `isSelected(value)` (líneas 29-31)

```ts
public isSelected(value: string): boolean {
  return this.formControl.value === value;
}
```

Compara cada opción con el valor actual del control.

Uso típico en HTML:
- aplicar clase activa a la tarjeta seleccionada (`[class.active]="isSelected(option.value)"`).

---

## 5) `buildOptionId(value)` (líneas 33-35)

```ts
public buildOptionId(value: string): string {
  return `serviceOption${value.charAt(0).toUpperCase()}${value.slice(1)}`;
}
```

Genera IDs dinámicos y legibles, por ejemplo:

- `"gas"` -> `serviceOptionGas`
- `"electricity"` -> `serviceOptionElectricity`

Para qué sirve:

- tests automáticos (`querySelector('#serviceOptionGas')`)
- accesibilidad y trazabilidad en DOM
- debugging más fácil

---

## Resumen técnico corto

Este componente:
- toma opciones desde `props`,
- pinta tarjetas,
- actualiza `formControl` al click,
- refleja visualmente la opción activa,
- genera IDs semánticos.

Y todo queda integrado con Formly/Reactive Forms sin lógica manual en el componente padre.

---

## 7) Registro del tipo custom (`formly-types.module.ts`)

### Pregunta
`@src/app/shared/formly-types/formly-types.module.ts:12-18 explicame`

### Respuesta
Ese bloque es donde **registras tu tipo personalizado de Formly**.

```ts
FormlyModule.forChild({
  types: [
    {
      name: 'service-selector',
      component: ServiceSelectorFieldComponent,
    },
  ],
})
```

## Qué hace exactamente

- `FormlyModule.forChild(...)`:
  - agrega configuración de Formly a nivel de este módulo.
- `types: [...]`:
  - lista de tipos custom que quieres declarar.
- `name: 'service-selector'`:
  - nombre que vas a usar en el JSON (`"type": "service-selector"`).
- `component: ServiceSelectorFieldComponent`:
  - componente Angular que Formly debe renderizar cuando vea ese `type`.

## Traducción simple

Le estás diciendo a Formly:

“Cuando en el JSON aparezca `type: 'service-selector'`, usa `ServiceSelectorFieldComponent` para dibujar ese campo.”

## Resultado en tu app

Gracias a esto, en `service-contract-form.json` puedes poner:

- `key: "service"`
- `type: "service-selector"`

y Formly muestra tus tarjetas personalizadas en lugar de un input/radio estándar.

---

## 8) Template del custom field (`service-selector-field.component.html`)

### Pregunta
`@src/app/shared/formly-types/service-selector-field/service-selector-field.component.html:1-21 explicame`

### Respuesta
Este template es la **vista del campo personalizado** `service-selector`.  
Renderiza opciones como tarjetas/botones, marca la seleccionada y muestra error si falta elegir.

## Explicación por partes

### Contenedor y título

- `<div class="service">`  
  Contenedor general del custom field.
- `<label class="title">{{ props.label }}</label>`  
  Muestra el label que viene desde el JSON de Formly (`props.label`, por ejemplo “Service type”).

---

### Lista de opciones (tarjetas)

Dentro de `<div class="options">` se pintan botones dinámicamente:

- `*ngFor="let option of serviceOptions"`  
  Recorre las opciones (`gas`, `electricity`) que vienen de `props.options`.

- `[id]="buildOptionId(option.value)"`  
  Genera id dinámico (`serviceOptionGas`, etc.) útil para tests/accesibilidad.

- `class="option"`  
  Clase base visual de cada tarjeta.

- `type="button"`  
  Importante: evita que el botón dispare submit del formulario.

- `[class.active]="isSelected(option.value)"`  
  Si la opción coincide con el valor actual del control, agrega clase `active` (estado seleccionado).

- `(click)="selectService(option.value)"`  
  Al hacer click, actualiza el `formControl` del campo (integra con Formly/Reactive Forms).

Contenido del botón:
- `<span class="name">{{ option.label }}</span>` -> texto principal (“Gas”, “Electricity”).
- `<span class="hint" *ngIf="option.hint">{{ option.hint }}</span>` -> descripción opcional.

---

### Mensaje de error

```html
<small class="error" *ngIf="showError">
  {{ props['requiredMessage'] || 'You must select a service type.' }}
</small>
```

- `*ngIf="showError"`: Formly decide cuándo mostrar error (campo inválido y tocado/submit).
- Muestra mensaje personalizado si existe `props.requiredMessage`.
- Si no viene, usa el texto por defecto.

---

## Qué logra este HTML en conjunto

- Lee configuración del JSON (`label`, `options`, `requiredMessage`).
- Presenta una UI personalizada (tarjetas clicables).
- Sigue funcionando como campo real del formulario (valor, validación, error) gracias a `FieldType` + `formControl`.

---

## 9) Template principal del formulario (`service-contract-form.component.html`)

### Pregunta
`@service-contract-form.component.html (1-38) explicame`

### Respuesta
Este HTML es la **pantalla del formulario principal**.  
No define los campos uno por uno; delega eso a Formly usando `fields` que vienen del JSON.

## Estructura general

- `<main id="serviceContractView" class="page">`
  - contenedor principal de la vista.
- `<section class="card">`
  - tarjeta visual donde vive el formulario.

## Formulario reactivo

```html
<form [formGroup]="form" (ngSubmit)="submitForm()" novalidate>
```

- `[formGroup]="form"`: conecta el formulario HTML con el `FormGroup` del componente.
- `(ngSubmit)="submitForm()"`: al enviar, ejecuta tu método `submitForm()`.
- `novalidate`: desactiva validación nativa del navegador para usar la de Angular/Formly.

## Mensaje de error de carga

```html
<p class="loadError" *ngIf="loadError">{{ loadError }}</p>
```

- Si falló cargar la config JSON, muestra el texto guardado en `loadError`.

## Render dinámico de campos con Formly

```html
<formly-form
  [form]="form"
  [fields]="fields"
  [model]="model"
  [options]="options">
</formly-form>
```

Este bloque es la clave:

- `[form]="form"`: el árbol de controles reactivos.
- `[fields]="fields"`: definición de campos (viene del JSON API/mock).
- `[model]="model"`: objeto con los valores actuales.
- `[options]="options"`: opciones avanzadas de Formly (ej. `resetModel`).

O sea: Formly lee `fields` y dibuja automáticamente `name`, `email`, `service`, etc.

## Bloque informativo según servicio seleccionado

```html
<div class="info" *ngIf="hasSelectedService">
  <span class="infoText">
    You are contracting {{ selectedServiceLabel }}.
  </span>
</div>
```

- Se muestra solo si ya eligieron servicio.
- Usa getters del componente:
  - `hasSelectedService` (boolean)
  - `selectedServiceLabel` (`Gas`/`Electricity`)

## Botones de acción

### Clear

```html
<button type="button" (click)="clearForm()">Clear</button>
```

- `type="button"` evita submit.
- llama `clearForm()` para limpiar formulario/modelo.

### Submit

```html
<button
  type="submit"
  [disabled]="form.invalid || !hasSelectedService">
  {{ submitLabel }}
</button>
```

- `type="submit"` dispara `ngSubmit`.
- se deshabilita si:
  - el form es inválido, o
  - no hay servicio elegido.
- texto dinámico del botón con `submitLabel` (viene del JSON).

---

En resumen: este template arma el “marco” del formulario, y `formly-form` se encarga de pintar los campos dinámicos definidos por configuración.

---

## 10) Servicio de carga de config (`form-config.service.ts`)

### Pregunta
`@src/app/core/services/form-config.service.ts:1-17 explicame a detalle`

### Respuesta
Perfecto. Este servicio es el encargado de **traer la configuración dinámica del formulario** (el JSON) para que luego Formly lo renderice.

## Código (qué es)

`FormConfigService` es una clase de Angular marcada como `@Injectable`, que usa `HttpClient` para hacer un `GET` al archivo JSON de configuración.

---

## Explicación línea por línea

- `import { HttpClient } from "@angular/common/http";`
  - importa cliente HTTP de Angular para hacer llamadas REST/GET.

- `import { Injectable } from "@angular/core";`
  - decorador que permite que la clase participe en el sistema DI (inyección de dependencias).

- `import { Observable } from "rxjs";`
  - tipo reactivo que representa un flujo asíncrono (la respuesta HTTP llegará después).

- `import { ServiceFormConfig } from "../../shared/models/service-form-config.model";`
  - tipo de datos esperado en la respuesta (`submitLabel` + `fields`).

---

- `@Injectable({ providedIn: 'root' })`
  - registra este servicio en el inyector raíz.
  - significa singleton global: una sola instancia para toda la app.
  - no necesitas agregarlo manualmente en `providers`.

---

- `export class FormConfigService {`
  - definición del servicio.

- `private readonly _serviceContractConfigUrl = '/api/public/mocks/v1/parameters/service-contract-form.json';`
  - URL del recurso a pedir.
  - `private`: solo se usa dentro del servicio.
  - `readonly`: no se cambia después de inicializar.

---

- `constructor(private readonly _http: HttpClient) {}`
  - Angular inyecta `HttpClient`.
  - ese objeto se guarda como `_http` para usarlo en métodos del servicio.

---

- `getServiceContractConfig(): Observable<ServiceFormConfig> {`
  - método público que devuelve un `Observable` tipado.
  - quien lo consume (`component`) se suscribe para recibir la config.

- `return this._http.get<ServiceFormConfig>(this._serviceContractConfigUrl);`
  - ejecuta un GET a la URL.
  - `<ServiceFormConfig>` tipa la respuesta esperada.
  - retorna el observable sin subscribirse aquí (buena práctica: el componente decide cuándo subscribirse).

---

## Cómo encaja en el flujo real

1. El componente llama `getServiceContractConfig()`.
2. Se suscribe en `ngOnInit`.
3. En `next`, asigna:
   - `this.submitLabel = config.submitLabel`
   - `this.fields = config.fields`
4. `formly-form` usa esos `fields` para dibujar el formulario dinámico.

---

## Por qué está bien diseñado

- **Responsabilidad única**: solo obtiene config, no mezcla UI.
- **Tipado fuerte**: evita inconsistencias del JSON.
- **Reutilizable**: cualquier componente podría usar la misma config.
- **Angular style correcto**: DI + Observable + servicio singleton.

---

## 11) JSON completo del formulario (`service-contract-form.json`)

### Pregunta
`@service-contract-form.json (1-166) explicame a detalle`

### Respuesta
Perfecto. Este JSON es **la definición completa del formulario dinámico**.  
Formly lo lee y con eso construye UI + validaciones + lógica condicional, sin hardcodear campos en HTML.

## Estructura global

- `submitLabel`: texto del botón de envío.
- `fields`: arreglo de campos Formly.  
  Cada objeto en `fields` define un campo del formulario.

---

## Cómo leer cada campo Formly (patrón)

En general, cada campo usa estas partes:

- `key`: nombre de la propiedad en `model`.
- `type`: tipo de control (`input`, `radio`, custom `service-selector`).
- `props`: configuración visual/funcional (label, required, options, min, etc.).
- `validators`: validadores (ej. `email`).
- `validation.messages`: mensajes de error.
- `expressions`: lógica dinámica en base al `model`.
- `resetOnHide`: si se oculta el campo, borra su valor.

---

## Campo por campo

### 1) `name` (líneas 4-18)

- `key: "name"` -> se guarda en `model.name`.
- `type: "input"` con `props.type: "text"`.
- obligatorio (`required: true`).
- mensaje: `"Name is required."`.

Resultado: input de texto para nombre.

---

### 2) `email` (líneas 19-39)

- `key: "email"` -> `model.email`.
- input tipo `email`.
- obligatorio.
- validador `"email"` en `validators.validation`.
- mensajes:
  - requerido
  - email inválido.

Resultado: exige formato de correo válido.

---

### 3) `service` custom (líneas 40-65)

- `key: "service"` -> `model.service`.
- `type: "service-selector"` (tu tipo personalizado).
- obligatorio.
- `options`:
  - Gas (`value: "gas"`)
  - Electricity (`value: "electricity"`)
- cada opción tiene `label`, `value`, `hint`.
- mensaje requerido personalizado.

Resultado: selector en tarjetas (no select estándar).

---

### 4) Bloque electricidad: `rateElectricity` (líneas 66-93)

- `key: "rateElectricity"` -> `model.rateElectricity`.
- `type: "radio"` con opciones `fixed` / `variable`.
- `resetOnHide: true`.
- `expressions`:
  - `hide`: oculto si `service` no es `'electricity'`.
  - `props.required`: obligatorio solo si `service` es `'electricity'`.
- mensaje requerido específico.

Resultado: solo aparece cuando eliges electricidad.

---

### 5) Bloque electricidad: `power` (líneas 94-114)

- `key: "power"` -> `model.power`.
- input numérico.
- `min: 1`.
- `resetOnHide: true`.
- mismas expresiones condicionales de electricidad.
- mensajes:
  - requerido
  - mínimo > 0.

Resultado: potencia contratada solo para electricidad.

---

### 6) Bloque gas: `rateGas` (líneas 115-142)

- `key: "rateGas"` -> `model.rateGas`.
- `type: "radio"` con opciones `tur` / `freeMarket`.
- `resetOnHide: true`.
- `expressions`:
  - oculto si `service` no es `'gas'`.
  - requerido solo si `service` es `'gas'`.
- mensaje requerido específico.

Resultado: tipo de tarifa solo para gas.

---

### 7) Bloque gas: `consumption` (líneas 143-163)

- `key: "consumption"` -> `model.consumption`.
- input numérico.
- `min: 1`.
- `resetOnHide: true`.
- visible/requerido solo cuando `service === 'gas'`.
- mensajes de requerido y mínimo.

Resultado: consumo estimado solo para gas.

---

## Lógica dinámica clave

`expressions` usa el estado actual del modelo (`model.service`) para decidir:

- **qué campos se muestran** (`hide`)
- **qué campos son obligatorios** (`props.required`)

Esto permite un solo formulario que cambia según selección del usuario.

---

## Por qué `resetOnHide: true` es muy importante

Cuando cambias de servicio (por ejemplo, de gas a electricidad):

- los campos ocultos del servicio anterior se limpian.
- evitas enviar “basura” o datos mezclados (ej. `rateGas` junto a `power`).

---

## Modelo que se va llenando (ejemplos)

Si elige gas, el `model` termina parecido a:

```json
{
  "name": "Ana",
  "email": "ana@mail.com",
  "service": "gas",
  "rateGas": "tur",
  "consumption": 120
}
```

Si elige electricidad:

```json
{
  "name": "Ana",
  "email": "ana@mail.com",
  "service": "electricity",
  "rateElectricity": "fixed",
  "power": 4.6
}
```

---

## Resumen técnico

Este JSON actúa como **contrato declarativo del formulario**:

- define UI (`type`, `props`),
- define validación (`required`, `validators`, `messages`),
- define comportamiento dinámico (`expressions`),
- y protege consistencia de datos (`resetOnHide`).

Si quieres, en el siguiente paso te lo puedo convertir a una “plantilla base” para que la reutilices en otros formularios (solo cambiando campos/opciones).
