# Guía completa del proyecto `practicar-formly`

Esta guía te deja todo listo para explicar el proyecto de punta a punta: arquitectura, flujo funcional y qué hace cada archivo de `src`, incluyendo una lectura línea por línea (o por bloques contiguos cuando la línea pertenece a la misma sentencia).

## 1) Visión general de la app

La app es un formulario dinámico de contratación de servicios construido con Angular + Reactive Forms + `ngx-formly`.

- El usuario completa `name` y `email`.
- El usuario elige `service` con un **custom field** de Formly (tarjetas clicables).
- Según el servicio (`gas` o `electricity`), Formly muestra/oculta campos dependientes.
- Al enviar, se valida y se construye un payload tipado para `console.log`.

## 2) Estructura de `src` (qué hay y para qué)

- `src/main.ts`: punto de arranque Angular.
- `src/index.html`: shell HTML base.
- `src/styles.scss`: estilos globales.
- `src/api/public/mocks/v1/parameters/service-contract-form.json`: configuración dinámica del formulario.
- `src/app/app.module.ts`: módulo raíz + configuración global de Formly.
- `src/app/app-routing.module.ts`: lazy loading del feature principal.
- `src/app/app.component.*`: componente raíz que contiene `router-outlet`.
- `src/app/core/services/form-config.service.ts`: servicio HTTP para traer config del formulario.
- `src/app/features/service-contract/*`: feature module y componente de formulario.
- `src/app/shared/models/*`: modelos y tipos TypeScript.
- `src/app/shared/formly-types/*`: registro y custom type `service-selector`.

## 3) Flujo end-to-end de ejecución

1. Angular arranca en `main.ts` y hace bootstrap de `AppModule`.
2. `AppRoutingModule` carga en lazy `ServiceContractModule` al entrar a `/`.
3. Se renderiza `ServiceContractFormComponent`.
4. En `ngOnInit`, el componente pide la config JSON al `FormConfigService`.
5. La respuesta llena `fields` y `submitLabel`.
6. `<formly-form>` renderiza los campos definidos en JSON.
7. El campo custom `service-selector` actualiza `formControl` al hacer click.
8. Las `expressions` de Formly muestran/ocultan campos y activan `required` dinámico.
9. Con `resetOnHide: true`, los valores de campos ocultos se limpian automáticamente.
10. En submit, si el form es válido, se transforma `model` a un payload tipado y se imprime en consola.

---

## 4) Explicación archivo por archivo (línea por línea)

## `src/main.ts`

- **L1**: importa `platformBrowserDynamic` para bootstrap en navegador.
- **L3**: importa el módulo raíz `AppModule`.
- **L6-L7**: inicializa Angular con `bootstrapModule(AppModule)` y captura errores en consola.

## `src/index.html`

- **L1**: declara documento HTML5.
- **L2**: define idioma base (`en`).
- **L3-L9**: metadata del documento (`charset`, `title`, `base href`, `viewport`, favicon).
- **L10-L12**: cuerpo con `<app-root></app-root>`, que Angular reemplaza por la app.
- **L13**: cierre del documento.

## `src/styles.scss`

- **L1-L4**: estilo global básico del `body` (sin margen y con tipografía).
- **L6-L9**: fuerza ancho completo para componentes PrimeNG/Formly en modo fluido.

## `src/app/app.module.ts`

- **L1-L2**: imports base de Angular (`NgModule`, `BrowserModule`).
- **L4-L10**: importa routing, componente raíz, HTTP, animaciones, Formly, módulo de tipos custom y `Validators`.
- **L12-L33**: configuración del módulo raíz.
  - **L13-L15**: declara `AppComponent`.
  - **L16-L30**: registra imports.
    - `BrowserModule`: base navegador.
    - `HttpClientModule`: permite llamadas HTTP.
    - `BrowserAnimationsModule`: soporte animaciones.
    - **L20-L27**: `FormlyModule.forRoot(...)` con validador global `email`.
    - `FormlyTypesModule`: carga custom type `service-selector`.
    - `AppRoutingModule`: rutas raíz.
  - **L31**: sin providers manuales.
  - **L32**: arranca con `AppComponent`.
- **L34**: exporta clase `AppModule`.
- **L35**: línea en blanco final.

## `src/app/app-routing.module.ts`

- **L1-L2**: imports de módulo y router.
- **L4-L12**: define rutas.
  - **L6**: ruta base `''`.
  - **L7-L10**: lazy loading de `ServiceContractModule`.
- **L14-L17**: `RouterModule.forRoot(routes)` y export del router.
- **L18**: exporta `AppRoutingModule`.

## `src/app/app.component.ts`

- **L1**: importa `Component`.
- **L3-L7**: metadata del componente raíz.
  - selector: `app-root`
  - template: `app.component.html`
  - estilo: `app.component.scss`
- **L8**: clase vacía `AppComponent` (solo contenedor de ruta).

## `src/app/app.component.html`

- **L1**: `<router-outlet>`; aquí Angular monta el componente de la ruta activa.

## `src/app/app.component.scss`

- Archivo vacío: no define estilos locales para `AppComponent`.

## `src/app/app.component.spec.ts`

> Este test es el generado por plantilla inicial y quedó desactualizado respecto al componente real.

- **L1-L3**: imports de testing y del `AppComponent`.
- **L5-L15**: setup del módulo de test.
- **L17-L21**: test válido: crea el componente.
- **L23-L27**: test inválido hoy: espera propiedad `title` que no existe en `AppComponent`.
- **L29-L34**: test inválido hoy: espera render de `<h1>` que tampoco existe.

## `src/app/core/services/form-config.service.ts`

- **L1-L4**: imports de HTTP, DI, RxJS y modelo `ServiceFormConfig`.
- **L6-L8**: `@Injectable({ providedIn: 'root' })` para singleton global.
- **L9**: declara clase `FormConfigService`.
- **L10**: URL del JSON mock del formulario.
- **L12**: inyecta `HttpClient`.
- **L14-L16**: método público que devuelve `Observable<ServiceFormConfig>` haciendo `GET`.
- **L17**: cierre de clase.

## `src/app/features/service-contract/service-contract.module.ts`

- **L1-L7**: imports del feature module, componente, Angular común, router, Formly y Reactive Forms.
- **L9-L24**: metadata de módulo.
  - **L10**: declara `ServiceContractFormComponent`.
  - **L11-L22**: importa módulos requeridos:
    - `CommonModule`
    - `ReactiveFormsModule`
    - `FormlyModule`
    - `FormlyPrimeNGModule`
    - **L16-L21**: ruta hija `''` que renderiza el formulario.
  - **L23**: exporta el componente (opcional para reutilizar).
- **L25**: clase `ServiceContractModule`.

## `src/app/features/service-contract/components/service-contract-form/service-contract-form.component.ts`

- **L1-L5**: imports de Angular, forms, Formly, RxJS y servicio de config.
- **L6-L11**: imports de tipos de dominio del formulario.
- **L13-L17**: metadata del componente.
- **L18**: clase con `OnInit` y `OnDestroy`.
- **L19**: `form` reactivo principal.
- **L20**: `model` de Formly (draft mutable).
- **L21**: `options` de Formly (incluye `resetModel`).
- **L22**: `fields` dinámicos (`FormlyFieldConfig[]`).
- **L23**: texto del botón submit.
- **L24**: mensaje de error de carga.
- **L26**: `Subject` para cortar suscripciones en destroy.
- **L28**: inyección del servicio.
- **L30-L45**: `ngOnInit`:
  - llama al servicio.
  - usa `takeUntil` para evitar leaks.
  - en `next`: setea `submitLabel` y `fields`.
  - en `error`: limpia campos y muestra error.
- **L47-L50**: `ngOnDestroy` completa `_destroy$`.
- **L52-L59**: `submitForm`:
  - si inválido o sin servicio: marca controles tocados y retorna.
  - si válido: `console.log` con payload transformado.
- **L61-L69**: `clearForm`:
  - si Formly expone `resetModel`, lo usa.
  - fallback: limpia `model` y `form`.
- **L71-L73**: getter `hasSelectedService`.
- **L75-L85**: getter `selectedServiceLabel` para texto visual (`Gas`/`Electricity`).
- **L87-L110**: `_buildSubmitPayload`:
  - extrae `name`, `email`, `service`.
  - `switch(service)`:
    - `gas`: retorna payload con `rateGas` y `consumption`.
    - `electricity`: retorna payload con `rateElectricity` y `power`.
  - `default`: lanza error si no hay servicio.
- **L111**: cierre de clase.

## `src/app/features/service-contract/components/service-contract-form/service-contract-form.component.html`

- **L1-L2**: layout principal (`main` + `section` tarjeta).
- **L3**: formulario reactivo con `ngSubmit`.
- **L4**: error de carga visible condicionalmente.
- **L6-L11**: `formly-form` enlazado con `form`, `fields`, `model`, `options`.
- **L13-L17**: bloque informativo visible solo si hay servicio seleccionado.
- **L19-L35**: acciones.
  - botón `Clear` (tipo botón) ejecuta `clearForm`.
  - botón `Submit` (tipo submit) deshabilitado cuando form inválido o sin servicio.
- **L36-L38**: cierres.

## `src/app/features/service-contract/components/service-contract-form/service-contract-form.component.scss`

- **L1-L9**: variables SCSS de colores.
- **L11-L18**: `.page` centra el formulario en viewport.
- **L20-L27**: `.card` define contenedor visual principal.
- **L29-L34**: `.loadError` para mensaje de error.
- **L36-L45**: `.info` para mensaje contextual del servicio elegido.
- **L47-L75**: `.actions` y estilos de botones (normal, hover, disabled).
- **L77-L81**: ajuste de ancho/margen para inputs PrimeNG dentro del componente.
- **L83-L88**: media query móvil para alinear `.info`.

## `src/app/features/service-contract/components/service-contract-form/service-contract-form.component.spec.ts`

> También generado por plantilla y hoy incompleto.

- **L1-L4**: imports de testing; `By`/`DebugElement` no se usan.
- **L6**: import del componente.
- **L8-L17**: setup del test module solo con `declarations`.
- **L19-L23**: crea fixture/componente y ejecuta `detectChanges`.
- **L25-L27**: único test: componente se crea.

## `src/app/shared/models/form-option.model.ts`

- **L1-L5**: interfaz `FormOption` para opciones del selector custom:
  - `label`: texto visible.
  - `value`: valor guardado.
  - `hint?`: descripción opcional.

## `src/app/shared/models/service-form-config.model.ts`

- **L1**: importa `FormlyFieldConfig`.
- **L3-L6**: interfaz del JSON de configuración:
  - `submitLabel: string`
  - `fields: FormlyFieldConfig[]`
- **L7-L9**: líneas en blanco finales.

## `src/app/shared/models/service-form.model.ts`

- **L1-L3**: tipos unión de dominio (`ServiceType`, `GasRateType`, `ElectricityRateType`).
- **L5-L13**: `ServiceFormDraft` (modelo editable del formulario, todo opcional).
- **L15-L19**: `ServiceFormBasePayload` base obligatorio.
- **L21-L25**: payload final para gas.
- **L27-L31**: payload final para electricidad.
- **L33-L35**: tipo unión `ServiceFormPayload` (gas o electricidad).
- **L36**: línea final.

## `src/app/shared/formly-types/formly-types.module.ts`

- **L1-L5**: imports de módulo y `ServiceSelectorFieldComponent`.
- **L7-L21**: `NgModule` del paquete de tipos custom.
  - **L8**: declara componente custom.
  - **L9-L20**: imports:
    - `CommonModule`
    - `ReactiveFormsModule`
    - **L12-L19**: `FormlyModule.forChild` registra tipo:
      - nombre: `service-selector`
      - componente: `ServiceSelectorFieldComponent`
- **L22**: exporta clase `FormlyTypesModule`.

## `src/app/shared/formly-types/service-selector-field/service-selector-field.component.ts`

- **L1-L3**: imports del componente, `FieldType` y `FormOption`.
- **L5-L9**: metadata del custom field.
- **L10**: extiende `FieldType` para integrarse con Formly.
- **L11-L13**: getter `serviceOptions` lee `props.options` del config.
- **L15-L27**: `selectService(value)`:
  - evita cambios si control deshabilitado.
  - evita setear el mismo valor.
  - actualiza `formControl`, marca `dirty` y `touched`.
- **L29-L31**: `isSelected(value)` para estado visual activo.
- **L33-L35**: `buildOptionId(value)` genera ids de botones (`serviceOptionGas`, etc).
- **L36-L37**: cierre.

## `src/app/shared/formly-types/service-selector-field/service-selector-field.component.html`

- **L1-L2**: contenedor y label del campo.
- **L4-L16**: renderizado de botones por `*ngFor`:
  - id dinámico,
  - clase activa según selección,
  - click que llama `selectService`,
  - nombre y hint de opción.
- **L18-L20**: mensaje de error cuando `showError` de Formly es verdadero.
- **L21**: cierre.

## `src/app/shared/formly-types/service-selector-field/service-selector-field.component.scss`

- **L1-L9**: variables de color.
- **L11-L16**: layout vertical del campo.
- **L17-L21**: estilo del título.
- **L23-L27**: grilla de 2 columnas para opciones.
- **L29-L51**: estilo de tarjeta opción:
  - borde, padding, tipografía,
  - hover,
  - estado `.active`.
- **L53-L56**: estilo de nombre de opción.
- **L58-L62**: estilo del hint.
- **L64-L66**: color de error.
- **L67**: cierre.

## Deep dive: componente personalizado `service-selector` (explicado fácil)

Esta parte es clave para tu exposición. Tu componente personalizado no es "un botón suelto": es un **control de formulario real** conectado a Formly y al `FormGroup`.

### Qué problema resuelve

El enunciado pide que "Tipo de servicio" no sea un `select` estándar.  
Tu solución crea un tipo nuevo de Formly (`service-selector`) que se ve como tarjetas clicables, pero internamente se comporta como cualquier campo reactivo.

### Cómo se conecta todo (cadena completa)

1. En el JSON defines un campo:
   - `key: "service"`
   - `type: "service-selector"`
   - `props.options` con `gas` y `electricity`.
2. Formly lee ese `type`.
3. En `FormlyTypesModule`, `FormlyModule.forChild` dice:  
   "`service-selector` se renderiza con `ServiceSelectorFieldComponent`".
4. Formly crea ese componente y le inyecta:
   - `formControl` (el control reactivo real),
   - `props` (label, required, options, etc),
   - estado de validación (`showError`, touched, invalid...).
5. Cuando el usuario hace click en una tarjeta, `selectService(value)` actualiza `formControl`.
6. Como `formControl` cambió, se actualiza automáticamente:
   - `model.service`,
   - validaciones,
   - y las expresiones de otros campos (`hide` / `props.required`).

### Qué hace cada método del componente custom

- `serviceOptions`:
  - toma `props.options` del JSON y lo tipa como `FormOption[]`.
  - sirve para que el HTML pinte las tarjetas con `*ngFor`.
- `selectService(value)`:
  - corta ejecución si el control está deshabilitado.
  - evita trabajo si el valor ya estaba seleccionado.
  - guarda el nuevo valor en `formControl`.
  - marca `dirty` y `touched` para que validaciones y UI reaccionen.
- `isSelected(value)`:
  - devuelve `true` si ese valor coincide con el del control.
  - en template activa la clase `.active` de la tarjeta seleccionada.
- `buildOptionId(value)`:
  - crea ids semánticos (`serviceOptionGas`, `serviceOptionElectricity`).
  - útil para tests y accesibilidad básica.

### Por qué este componente dispara la lógica condicional del resto del formulario

Los campos dependientes usan expresiones como:

- `hide: model.service !== 'electricity'`
- `props.required: model.service === 'electricity'`

Entonces, cuando el custom field setea `formControl` con `gas` o `electricity`, Formly recalcula esas expresiones.  
Resultado: aparecen/desaparecen campos y cambian sus validaciones sin escribir lógica manual en el componente padre.

### Por qué `showError` funciona en el HTML del custom field

`ServiceSelectorFieldComponent` hereda de `FieldType`, que ya trae estado de error integrado con Formly.  
Por eso en el template puedes usar:

- `*ngIf="showError"`

y Formly decide cuándo mostrarlo (por ejemplo, campo requerido no completado tras interacción o submit).

### Resumen para decirlo en clase (30 segundos)

"Creé un tipo custom de Formly llamado `service-selector`. Lo registré en `FormlyModule.forChild` y lo uso en el JSON con `type: 'service-selector'`. El componente renderiza tarjetas, pero actualiza un `formControl` real. Al cambiar ese valor (`model.service`), Formly reevalúa las expresiones de los campos dependientes y gestiona automáticamente visibilidad, requeridos y errores."

## `src/app/shared/formly-types/service-selector-field/service-selector-field.component.spec.ts`

> Test base generado por plantilla y sin casos funcionales.

- **L1-L4**: imports de testing; `By`/`DebugElement` no se usan.
- **L6**: import del componente.
- **L8-L17**: setup de TestBed.
- **L19-L23**: creación de fixture/componente.
- **L25-L27**: test de creación.

## `src/api/public/mocks/v1/parameters/service-contract-form.json`

Este archivo es el corazón dinámico del formulario.

- **L1-L3**: abre objeto raíz con `submitLabel` y `fields`.
- **L4-L18**: campo `name`:
  - `type: input`, `required: true`, mensaje `"Name is required."`.
- **L19-L39**: campo `email`:
  - input email obligatorio,
  - validador `email` (definido globalmente en `AppModule`),
  - mensajes de error custom.
- **L40-L65**: campo custom `service`:
  - `type: service-selector`,
  - opciones `gas`/`electricity`,
  - mensaje requerido.
- **L66-L93**: `rateElectricity`:
  - `radio`,
  - `resetOnHide: true`,
  - visible solo con `model.service === 'electricity'`,
  - `required` dinámico cuando visible.
- **L94-L114**: `power`:
  - input numérico con `min: 1`,
  - misma lógica condicional para electricidad.
- **L115-L142**: `rateGas`:
  - `radio`,
  - visible solo para `gas`,
  - requerido dinámico.
- **L143-L163**: `consumption`:
  - input numérico con `min: 1`,
  - visible/requerido solo para gas.
- **L164-L165**: cierre de `fields` y objeto raíz.
- **L166**: línea final.

---

## 5) Cómo explicar el comportamiento dinámico en una exposición

Puedes usar este guion corto:

1. "El formulario no está hardcodeado en HTML, lo define un JSON de Formly."
2. "El selector de servicio es un custom field (`service-selector`) hecho con tarjetas."
3. "Cuando cambia `service`, Formly evalúa `expressions.hide` y `expressions.props.required`."
4. "Los campos ocultos se limpian con `resetOnHide: true`, evitando enviar basura."
5. "En submit, convierto el `model` a un payload final tipado según gas/electricidad."

## 6) Estado técnico actual (importante para defender la app)

- La funcionalidad principal del formulario dinámico está correctamente resuelta.
- Hay tests generados por plantilla que no reflejan el estado actual (`app.component.spec.ts`).
- Los specs del feature/custom field son mínimos (solo "should create"), no validan lógica condicional todavía.

Si quieres, en un siguiente paso te puedo preparar una segunda guía solo de testing (`GUIA_TESTS.md`) con los tests exactos que conviene agregar para defender la práctica al 100%.
