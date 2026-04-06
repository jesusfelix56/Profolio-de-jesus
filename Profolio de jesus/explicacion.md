# Guia completa del proyecto (paso a paso, sin repeticiones)

Esta guia explica **todo `src`** en un flujo unico de ejecucion.  
Regla de lectura: cada archivo aparece una sola vez con su funcion real, lo que exporta, sus variables/metodos y sus conexiones.

---

## 1) Arranque de la aplicacion

### `src/index.html`
- **Rol:** documento host de la SPA.
- **Clave:** contiene `<app-root>` (ancla donde Angular monta la app), `<base href="/">` (routing) y favicon.
- **Exporta/metodos:** no aplica (archivo estatico).

### `src/main.ts`
- **Rol:** entry point ejecutable.
- **Hace:** `platformBrowserDynamic().bootstrapModule(AppModule)` y captura errores con `.catch(...)`.
- **Conecta con:** `src/app/app.module.ts`.

---

## 2) Nucleo Angular raiz

### `src/app/app.module.ts`
- **Exporta:** `AppModule`.
- **Declara:** `AppComponent`.
- **Importa y habilita:**
  - navegador/animaciones (`BrowserModule`, `BrowserAnimationsModule`),
  - HTTP (`HttpClientModule`),
  - routing raiz (`AppRoutingModule`),
  - toast global (`ToastModule`),
  - backend mock (`HttpClientInMemoryWebApiModule.forRoot(InMemoryDataService, { dataEncapsulation: false })`).
- **Providers:** `MessageService` (base de toasts).

### `src/app/app-routing.module.ts`
- **Exporta:** `AppRoutingModule`.
- **Rutas raiz:**
  - `''` -> lazy `PublicModule`,
  - `'login'` -> lazy `AuthModule`,
  - `'admin'` -> lazy `AdminModule` con `canActivate: [authGuard]`.

### `src/app/app.component.ts`
- **Exporta:** `AppComponent`.
- **Variables:**
  - `isAuthenticated` (estado auth en UI),
  - `_authSub?` (suscripcion para limpiar).
- **Metodos:**
  - `ngOnInit()` -> subscribe a `AuthService.isAuthenticated$`.
  - `logout()` -> `AuthService.logout()` + `Router.navigate(['/contacts'])`.
  - `ngOnDestroy()` -> unsubscribe.

### `src/app/app.component.html`
- **Rol:** shell global.
- **Contiene:** nav principal, boton login/logout condicional, `<router-outlet>`, `<p-toast>`.
- **Eventos:** `(click)="logout()"`.

### `src/app/app.component.scss`
- **Rol:** estilos del shell (header/nav/main responsive).

### `src/app/app.component.spec.ts`
- **Rol:** test basico del root component (creacion y render de marca).

---

## 3) Capa core y datos mock

### `src/app/core/guards/auth.guard.ts`
- **Exporta:** `authGuard` (`CanActivateFn`).
- **Flujo:** si `AuthService.isAuthenticated()` es `true`, deja pasar; si no, redirige a `/login` y bloquea ruta.

### `src/app/core/api/mocks/mock-contacts.ts`
- **Exporta:** `CONTACTS: Contact[]` (dataset semilla).

### `src/app/services/in-memory-data.service.ts`
- **Exporta:** `InMemoryDataService`.
- **Metodos:**
  - `createDb()` -> expone `{ contacts }` desde `CONTACTS`.
  - `genId(contacts)` -> siguiente id incremental.
- **Conexion:** backend simulado para todas las llamadas `api/contacts`.

---

## 4) Modelos compartidos y validacion

### `src/app/shared/interfaces/contact.interface.ts`
- **Exporta:**
  - `interface Contact`,
  - `type ContactSortField`,
  - `interface ContactSortOption`,
  - `interface ContactColumnOption`.
- **Rol:** contrato tipado comun para servicios y componentes.

### `src/app/shared/utils/contact-validation.ts`
- **Exporta constantes:**
  - `CONTACT_EMAIL_PATTERN`,
  - `CONTACT_PHONE_PATTERN`.
- **Exporta funciones:**
  - `isContactEmailValid(value)`,
  - `isContactPhoneValid(value)`,
  - `isContactBodyValid(contact)` (requiere firstName/lastName/jobTitle + phone/email validos; `address` opcional),
  - `getContactFormFieldLabel(field)`,
  - `getContactFormFieldError(field, formModel, formSubmitted)`.
- **Rol:** reglas unificadas de validacion para admin/formularios.

---

## 5) Servicios de aplicacion

### `src/app/services/auth.service.ts`
- **Exporta:** `AuthService`.
- **Variables:**
  - `_isAuthenticated$` (BehaviorSubject),
  - `_adminUsername = 'admin'`,
  - `_adminPassword = 'admin123'`,
  - `isAuthenticated$` (observable publico).
- **Metodos:**
  - `login(username, password): Observable<boolean>` -> valida credenciales y actualiza subject.
  - `logout(): void` -> emite `false`.
  - `isAuthenticated(): boolean` -> lectura sincronica del estado actual.

### `src/app/services/contact.service.ts`
- **Exporta:** `ContactService`.
- **Variables:**
  - `_contactsUrl = 'api/contacts'`,
  - `_contacts$ = new BehaviorSubject<Contact[]>([])`,
  - `_httpOptions` (headers JSON).
- **Constructor:** llama `_loadContacts()`.
- **Metodos publicos:**
  - `getContacts()` -> stream de contactos.
  - `addContact(contact)` -> POST + refresh.
  - `updateContact(contact)` -> PUT + refresh.
  - `deleteContact(id)` -> DELETE + refresh.
- **Metodos privados:**
  - `_loadContacts()` -> GET y `next` del subject.
  - `_refreshContacts<T>()` -> `tap` para recargar tras mutacion.
  - `_handleError<T>(...)` -> fallback con `of(...)`.
- **RxJS usado:** `BehaviorSubject`, `tap`, `catchError`, `of`.

### `src/app/services/app-toast.service.ts`
- **Exporta:** `AppToastService`.
- **Metodos:**
  - `success(summary, detail)`,
  - `error(summary, detail)`,
  - `info(summary, detail)`,
  - `_show(severity, summary, detail)` (privado).
- **Conexion:** centraliza toasts PrimeNG usando `MessageService`.

### `src/app/services/clipboard-toast.service.ts`
- **Exporta:** `ClipboardToastService`.
- **Metodo:** `copyWithFeedback(value, label)`.
  - limpia valor,
  - copia al portapapeles si hay API,
  - muestra toast de feedback.
- **Consumidores:** tarjetas y modal de perfil publico.

---

## 6) Feature Auth (login)

### `src/app/features/auth/auth-routing.module.ts`
- **Rol:** ruta interna del modulo auth.
- **Ruta:** `''` -> `LoginComponent`.

### `src/app/features/auth/auth.module.ts`
- **Declara:** `LoginComponent`.
- **Importa:** `FormsModule`, `AuthRoutingModule` y PrimeNG de formulario/card.

### `src/app/features/auth/components/login/login.component.ts`
- **Variables:** `username`, `password`, `errorMessage`.
- **Metodo `login()`:**
  - llama `AuthService.login(...)`,
  - si invalido: setea `errorMessage`,
  - si valido: limpia error, toast bienvenida y navega a `/admin/contacts`.

### `src/app/features/auth/components/login/login.component.html`
- **Bindings:** `[(ngModel)]` en usuario/password, `*ngIf="errorMessage"`.
- **Evento:** boton `Sign in` -> `login()`.

### `src/app/features/auth/components/login/login.component.scss`
- **Rol:** estilos del formulario y del mensaje de error.

---

## 7) Feature Public (listado de contactos)

### `src/app/features/public/public-routing.module.ts`
- `''` redirige a `contacts`; `'contacts'` renderiza `ContactListComponent`.

### `src/app/features/public/public.module.ts`
- **Declara:** `ContactListComponent`, `ContactListFiltersComponent`, `ContactCardComponent`, `ContactProfileDialogComponent`.
- **Importa:** `FormsModule` + PrimeNG para lista/filtros/dialog/tarjetas.

### `src/app/features/public/components/contact-list/contact-list.component.ts`
- **Rol:** contenedor de la vista publica.
- **Variables:**
  - `contacts`, `filteredContacts`,
  - `selectedContact`, `profileVisible`,
  - `loading`, `searchTerm`, `selectedSort`, `sortOptions`,
  - `_contactsSub?`.
- **Metodos:**
  - `ngOnInit()` -> subscribe a contactos, aplica filtros, apaga loading.
  - `onSearchChange(value)` y `onSortChange(value)` -> recalculan vista.
  - `clearFilters()` -> reset de filtros.
  - `openProfile(contact)` / `closeProfile()`.
  - `ngOnDestroy()` -> cleanup.
  - `_applyFilters()` -> filtra por nombre/apellido/fullName/telefono y ordena por campo seleccionado.

### `src/app/features/public/components/contact-list/contact-list.component.html`
- integra:
  - `app-contact-list-filters`,
  - skeletons cuando `loading`,
  - `p-dataView` paginado con `filteredContacts`,
  - estado vacio con boton `Clear filters`,
  - `app-contact-profile-dialog`.

### `src/app/features/public/components/contact-list/contact-list.component.scss`
- estilos de layout/grid y estados visuales loading/empty.

### `src/app/features/public/components/contact-list/components/contact-list-filters/contact-list-filters.component.ts`
- **Inputs:** `searchTerm`, `selectedSort`, `sortOptions`.
- **Outputs:** `searchTermChange`, `selectedSortChange`.
- **Rol:** componente presentacional de filtros.

### `src/app/features/public/components/contact-list/components/contact-list-filters/contact-list-filters.component.html`
- input de busqueda y dropdown de orden con `ngModel` y `ngModelChange`.

### `src/app/features/public/components/contact-list/components/contact-list-filters/contact-list-filters.component.scss`
- estilos responsive del bloque de filtros.

### `src/app/features/public/components/contact-list/components/contact-card/contact-card.component.ts`
- **Input:** `contact`.
- **Output:** `viewProfile`.
- **Variables:** `defaultAvatar`, `_namePalette`, `_tagSeverities`.
- **Metodos:**
  - `openProfile()` -> emite contacto al padre.
  - `getNameColor()` / `getTagSeverity()` -> apariencia derivada de `id`.
  - `copyValue(value, label)` -> delega en `ClipboardToastService`.

### `src/app/features/public/components/contact-list/components/contact-card/contact-card.component.html`
- tarjeta con avatar, nombre, jobTitle, phone/email y boton `View profile`.

### `src/app/features/public/components/contact-list/components/contact-card/contact-card.component.scss`
- estilos visuales de card y lineas de informacion.

### `src/app/features/public/components/contact-list/components/contact-profile-dialog/contact-profile-dialog.component.ts`
- **Inputs:** `visible`, `contact`.
- **Outputs:** `visibleChange`, `close`.
- **Variable:** `defaultAvatar`.
- **Metodos:**
  - `onClose()` -> emite cierre.
  - `copyValue(value, label)` -> copy + toast.

### `src/app/features/public/components/contact-list/components/contact-profile-dialog/contact-profile-dialog.component.html`
- modal (`p-dialog`) con datos completos y botones de copiar.

### `src/app/features/public/components/contact-list/components/contact-profile-dialog/contact-profile-dialog.component.scss`
- estilos del modal de perfil.

---

## 8) Feature Admin (CRUD completo)

### `src/app/features/admin/admin-routing.module.ts`
- `'contacts'` -> `AdminContactsComponent`.
- `''` -> redireccion a `contacts`.

### `src/app/features/admin/admin.module.ts`
- **Declara:**
  - `AdminContactsComponent`,
  - `AdminContactsToolbarComponent`,
  - `AdminContactsTableComponent`,
  - `ContactFormDialogComponent`,
  - `ConfirmDeleteDialogComponent`.
- **Importa:** `FormsModule` + PrimeNG de tabla/dialog/inputs/multiselect/tooltip.

### `src/app/features/admin/components/admin-contacts/admin-contacts.component.ts`
- **Rol:** orquestador central del CRUD.
- **Variables:**
  - datos/seleccion: `contacts`, `selectedContact`, `selectedContacts`,
  - visibilidad: `contactDialogVisible`, `deleteDialogVisible`,
  - modo: `deleteDialogMode ('single' | 'bulk')`,
  - columnas: `columns`, `selectedColumns`,
  - formulario: `formModel`, `formSubmitted`,
  - textos dialogo: `deleteDialogTitle`, `deleteDialogMessage`, `deleteDialogConfirmLabel`,
  - internos: `_contactsSub?`, `_clonedContacts`, `_contactsTableComponent?`.
- **Metodos:**
  - `ngOnInit()` -> subscribe a contactos y orden.
  - `openCreate()` -> reset + abrir formulario.
  - `save()` -> valida (`isContactBodyValid`) y crea contacto.
  - `openDelete(contact)` -> configura borrado single.
  - `openDeleteSelected()` -> configura borrado bulk.
  - `confirmDeleteDialog()` -> ejecuta single o bulk (`forkJoin`), limpia estado y notifica.
  - `onRowEditInit(contact)` -> clona fila.
  - `onRowEditSave(contact)` -> valida y persiste; rollback si invalido.
  - `onRowEditCancel(contact)` -> restaura clon.
  - `updateFormModel(model)` -> sincroniza cambios del hijo.
  - `closeFormDialog()` -> cierra modal y limpia submit.
  - `onGlobalFilterChange(value)` -> delega a tabla por `ViewChild`.
  - `_emptyForm()` y `_showSuccess(...)` (helpers privados).
  - `ngOnDestroy()` -> unsubscribe.

### `src/app/features/admin/components/admin-contacts/admin-contacts.component.html`
- integra toolbar + tabla + dialog formulario + dialog confirmacion.
- conecta todos los Inputs/Outputs del flujo admin.

### `src/app/features/admin/components/admin-contacts/admin-contacts.component.scss`
- layout base de la vista admin.

### `src/app/features/admin/components/admin-contacts/components/admin-contacts-toolbar/admin-contacts-toolbar.component.ts`
- **Inputs:** `selectedCount`, `columns`, `selectedColumns`.
- **Outputs:** `addContact`, `deleteSelected`, `selectedColumnsChange`, `globalFilterChange`.
- **Metodos:** `onColumnsChange(...)`, `onGlobalFilterInput(...)`.

### `src/app/features/admin/components/admin-contacts/components/admin-contacts-toolbar/admin-contacts-toolbar.component.html`
- boton Add, barra bulk condicional, buscador global y selector de columnas.

### `src/app/features/admin/components/admin-contacts/components/admin-contacts-toolbar/admin-contacts-toolbar.component.scss`
- estilos responsive de toolbar.

### `src/app/features/admin/components/admin-contacts/components/admin-contacts-table/admin-contacts-table.component.ts`
- **Inputs:** `contacts`, `selectedColumns`, `selectedContacts`.
- **Outputs:** `selectedContactsChange`, `rowEditInit`, `rowEditSave`, `rowEditCancel`, `deleteContact`.
- **Interno:** `_dataTable?` (`ViewChild('dt')`).
- **Metodos:**
  - `applyGlobalFilter(value)` -> filtro global PrimeNG.
  - `clearSelectionOnFilter()` -> limpia seleccion al filtrar.

### `src/app/features/admin/components/admin-contacts/components/admin-contacts-table/admin-contacts-table.component.html`
- `p-table` con seleccion multiple, columnas dinamicas, edicion inline y acciones por fila.

### `src/app/features/admin/components/admin-contacts/components/admin-contacts-table/admin-contacts-table.component.scss`
- estilos de tabla, filas alternas y acciones.

### `src/app/features/admin/components/dialogs/contact-form-dialog/contact-form-dialog.component.ts`
- **Inputs:** `visible`, `formModel`, `formSubmitted`.
- **Outputs:** `visibleChange`, `save`, `cancel`, `formModelChange`.
- **Metodos:**
  - `close()`,
  - `updateField(field, value)`,
  - `isFieldInvalid(field)`,
  - `getFieldError(field)`,
  - `getFieldLabel(field)`.

### `src/app/features/admin/components/dialogs/contact-form-dialog/contact-form-dialog.component.html`
- formulario de contacto con errores por campo y acciones Save/Cancel.

### `src/app/features/admin/components/dialogs/contact-form-dialog/contact-form-dialog.component.scss`
- estilos de grilla de formulario y estados de error.

### `src/app/features/admin/components/dialogs/confirm-delete-dialog/confirm-delete-dialog.component.ts`
- **Inputs:** `visible`, `title`, `message`, `confirmLabel`.
- **Outputs:** `visibleChange`, `confirm`.
- **Rol:** presentacional; la logica de borrado vive en el contenedor.

### `src/app/features/admin/components/dialogs/confirm-delete-dialog/confirm-delete-dialog.component.html`
- modal de confirmacion con botones Cancel/Confirm.

### `src/app/features/admin/components/dialogs/confirm-delete-dialog/confirm-delete-dialog.component.scss`
- estilos del modal de confirmacion.

---

## 9) Assets y estilos globales

### `src/assets/styles/_variables.scss`
- tokens SCSS de color, espaciado, bordes, radios y sombras.
- consumido por estilos globales y componentes.

### `src/styles.scss`
- hoja de estilo global:
  - base tipografica/layout general,
  - overrides visuales para PrimeNG/dialogs/tablas.

### `src/assets/default-user.svg`
- avatar fallback para contactos.

### `src/assets/.gitkeep`
- mantiene la carpeta `assets` versionada en git.

### `src/favicon.ico`
- icono de la pestana del navegador.

---

## 10) Resumen de flujo funcional completo

1. Arranca Angular (`index.html` + `main.ts` + `AppModule`).
2. Router raiz decide modulo por URL.
3. Auth controla acceso admin con `authGuard`.
4. `ContactService` mantiene estado reactivo de contactos y habla con backend mock.
5. Public muestra listado/filtros/perfil (solo lectura + copy).
6. Admin permite CRUD completo con validacion y dialogs.
7. Toasts comunican feedback global.
8. Estilos globales + assets cierran la experiencia visual.
