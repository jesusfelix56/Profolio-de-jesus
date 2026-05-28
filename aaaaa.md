"app": {
"api": {

"mockedLevel": "any",
"mockedLevel": "all",
"mocksRestBase": "http://localhost:3000/mocks/",
"-defaultRestBase": "https://homeplanner.santander.dev.corp/paas/homeplanner/bff/",
"-officeRestBase": "https://homeplanner.santander.dev.corp/web/homeplanner/bff/",
diff --git a/api/public/mocks/v1/customer-modification/clients.json b/api/public/mocks/v1/customer-modification/clients.json
new file mode 100644
index 00000000..6d18582c
--- /dev/null
+++ b/api/public/mocks/v1/customer-modification/clients.json
@@ -0,0 +1,41 @@
+[
{
"id": 1,
"fullName": "Jesús Félix",
"document": "12345678A",
"email": "jesus@test.com",
"phone": "600123123",
"accountNumber": "ES6621000418401234567891",
"accountType": "Cuenta Nómina",
"branchOffice": "Madrid Centro",
"transferLimit": 3000,
"notificationsEnabled": false,
"preferredContactMethod": "EMAIL"
},
{
"id": 2,
"fullName": "María García López",
"document": "87654321B",
"email": "maria.garcia@test.com",
"phone": "611234567",
"accountNumber": "ES7620770024003102575766",
"accountType": "Cuenta Ahorro",
"branchOffice": "Barcelona Norte",
"transferLimit": 1500,
"notificationsEnabled": false,
"preferredContactMethod": "PHONE"
},
{
"id": 3,
"fullName": "Carlos Ruiz Martínez",
"document": "11223344C",
"email": "carlos.ruiz@test.com",
"phone": "622345678",
"accountNumber": "ES9121000418450200051332",
"accountType": "Cuenta Empresa",
"branchOffice": "Sevilla Este",
"transferLimit": 2000,
"notificationsEnabled": false,
"preferredContactMethod": "SMS"
}
+]
diff --git a/api/public/mocks/v1/customer-modification/in-progress-context.json b/api/public/mocks/v1/customer-modification/in-progress-context.json
new file mode 100644
index 00000000..9b3f54fa
--- /dev/null
+++ b/api/public/mocks/v1/customer-modification/in-progress-context.json
@@ -0,0 +1,15 @@
+{
"client": {
"id": 1,
"fullName": "Jes\u00fas F\u00e9lix",
"document": "12345678A",
"email": "jesus@test.com",
"phone": "600123123",
"accountNumber": "ES6621000418401234567891",
"accountType": "Cuenta N\u00f3mina",
"branchOffice": "Madrid Centro",
"transferLimit": 3000,
"notificationsEnabled": false,
"preferredContactMethod": "EMAIL"
}
+}
\ No newline at end of file
diff --git a/api/public/mocks/v1/mortgage_originations/validate_existing_application/0049.json b/api/public/mocks/v1/mortgage_originations/validate_existing_application/0049.json
index 7471f2f5..fe20413e 100644
--- a/api/public/mocks/v1/mortgage_originations/validate_existing_application/0049.json
+++ b/api/public/mocks/v1/mortgage_originations/validate_existing_application/0049.json
@@ -1,10 +1,11 @@
{
"mortgageOrigination": null,
"isMortgageActive": false,
"mortgageOrigination": 1111,
"isMortgageActive": true,
"isPreGranted": true,
"preGrantedAmount": {
"amount": 125999.000,
"currency": "Eur"
},
"isNovation": false
-}
"isNovation": false,
"isCustomerModification": true
+}
\ No newline at end of file
diff --git a/api/public/mocks/v1/parameters/1.json b/api/public/mocks/v1/parameters/1.json
index 11f90ffa..fb6cee02 100644
--- a/api/public/mocks/v1/parameters/1.json
+++ b/api/public/mocks/v1/parameters/1.json
@@ -4027,6 +4027,17 @@
}
}
},
{
"tealium": {
"key": "distributor.events",
"parentKey": "clickElement"
},
"hideExpression": false,
"icon": "edit",
"title": "DISTRIBUTOR.BE_INTERESTED.CUSTOMER_MODIFICATION.TITLE",
"description": "DISTRIBUTOR.BE_INTERESTED.CUSTOMER_MODIFICATION.SUMMARY",
"path": "/customer-modification"
},
{
"tealium": {
"key": "distributor.events",
@@ -4175,6 +4186,392 @@
}
]
},
"customerModification": {
"form": {
"fields": [
{
"id": "customerModification-form",
"type": "stepper",
"templateOptions": {
"cancelButton": {
"type": "eventClient",
"eventClient": "cancelCustomerModification"
},
"stepsLabels": [
{
"step": 1,
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.STEPS_LABELS.LABEL_1"
},
{
"step": 2,
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.STEPS_LABELS.LABEL_2"
},
{
"step": 3,
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.STEPS_LABELS.LABEL_3"
}
],
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.LABEL",
"submitButtonText": "ACTIONS.REQUEST_MODIFICATION",
"icon": "next-arrow-icon-button"
},
"fieldGroup": [
{
"templateOptions": {
"buttons": [
{
"text": "ACTIONS.CONTINUE",
"type": "eventClient",
"eventClient": "loadModificationFormStep"
}
]
},
"fieldGroup": [
{
"id": "customerModificationSelectHeader",
"type": "title",
"className": "d-block mt-1",
"templateOptions": {
"variant": "headline",
"size": "small",
"textSlot": "CUSTOMER_MODIFICATION.FORM.FIELDS.SELECT_CLIENT.TITLE"
}
},
{
"id": "customerModificationSelectDescription",
"type": "text",
"className": "d-block mt-1 mb-1",
"templateOptions": {
"textSlot": "CUSTOMER_MODIFICATION.FORM.FIELDS.SELECT_CLIENT.DESCRIPTION"
}
},
{
"id": "customerModificationSelectClient",
"key": "selectedClientId",
"type": "customer-selection-radio",
"templateOptions": {
"required": true
}
}
]
},
{
"templateOptions": {
"buttons": [
{
"text": "ACTIONS.CONTINUE",
"type": "eventClient",
"eventClient": "loadSummaryStep"
}
]
},
"fieldGroupClassName": "row",
"fieldGroup": [
{
"id": "customerModificationDataHeader",
"type": "title",
"className": "col-xs-12 mt-1",
"templateOptions": {
"variant": "headline",
"size": "small",
"textSlot": "CUSTOMER_MODIFICATION.FORM.FIELDS.MODIFY_DATA.TITLE"
}
},
{
"id": "customerModificationDataDescription",
"type": "text",
"className": "col-xs-12 mt-1 mb-1",
"templateOptions": {
"textSlot": "CUSTOMER_MODIFICATION.FORM.FIELDS.MODIFY_DATA.DESCRIPTION"
}
},
{
"id": "customerModificationFullName",
"key": "fullName",
"type": "custom-input",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"placeholder": " ",
"required": true,
"sizeText": "small",
"labelText": "CUSTOMER_MODIFICATION.FORM.FIELDS.FULL_NAME.LABEL",
"maxLength": 50
},
"validators": {
"validation": [
{
"name": "custom",
"options": {
"evalFunction": "!value || !/\d/.test(value)",
"message": "CUSTOMER_MODIFICATION.VALIDATORS.NO_NUMBERS"
}
}
]
}
},
{
"id": "customerModificationEmail",
"key": "email",
"type": "custom-input",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"placeholder": " ",
"required": true,
"sizeText": "small",
"labelText": "CUSTOMER_MODIFICATION.FORM.FIELDS.EMAIL.LABEL"
},
"validators": {
"validation": [
{
"name": "custom",
"options": {
"evalFunction": "!value || /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/.test(value)",
"message": "CUSTOMER_MODIFICATION.VALIDATORS.EMAIL_FORMAT"
}
}
]
}
},
{
"id": "customerModificationPhone",
"key": "phone",
"type": "custom-input",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"placeholder": " ",
"required": true,
"sizeText": "small",
"labelText": "CUSTOMER_MODIFICATION.FORM.FIELDS.PHONE.LABEL",
"maxLength": 9
},
"validators": {
"validation": [
{
"name": "custom",
"options": {
"evalFunction": "!value || /^\d+$/.test(String(value))",
"message": "CUSTOMER_MODIFICATION.VALIDATORS.ONLY_NUMBERS"
}
},
{
"name": "custom",
"options": {
"evalFunction": "!value || String(value).replace(/\D/g, '').length <= 9",
"message": "CUSTOMER_MODIFICATION.VALIDATORS.MAX_NINE_DIGITS"
}
}
]
}
},
{
"id": "customerModificationAccountNumber",
"key": "accountNumber",
"type": "custom-input",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"placeholder": " ",
"required": true,
"sizeText": "small",
"labelText": "CUSTOMER_MODIFICATION.FORM.FIELDS.ACCOUNT_NUMBER.LABEL",
"minLength": 15,
"maxLength": 34
},
"validators": {
"validation": [
{
"name": "custom",
"options": {
"evalFunction": "!value || /^[A-Z]{2}[0-9]{2}[A-Z0-9]{11,30}$/.test(String(value).toUpperCase())",
"message": "CUSTOMER_MODIFICATION.VALIDATORS.IBAN_FORMAT"
}
}
]
}
},
{
"id": "customerModificationAccountType",
"key": "accountType",
"type": "searchable-modal",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"required": true,
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.ACCOUNT_TYPE.LABEL",
"modalTemplateOptions": {
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.ACCOUNT_TYPE.LABEL",
"placeholder": "ACTIONS.SEARCH",
"searchable": true
}
},
"expressionProperties": {
"templateOptions_options": "formState.selectOptionsData.accountTypeOptions"
}
},
{
"id": "customerModificationBranchOffice",
"key": "branchOffice",
"type": "searchable-modal",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"required": true,
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.BRANCH_OFFICE.LABEL",
"modalTemplateOptions": {
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.BRANCH_OFFICE.LABEL",
"placeholder": "ACTIONS.SEARCH",
"searchable": true
}
},
"expressionProperties": {
"templateOptions_options": "formState.selectOptionsData.branchOfficeOptions"
}
},
{
"id": "customerModificationTransferLimit",
"key": "transferLimit",
"type": "numeric-input-with-controls",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"type": "number",
"placeholder": " ",
"required": true,
"sizeText": "small",
"labelText": "CUSTOMER_MODIFICATION.FORM.FIELDS.TRANSFER_LIMIT.LABEL",
"numberDelta": "1",
"decimals": 0
},
"validators": {
"validation": [
{
"name": "custom",
"options": {
"evalFunction": "value === '' || value === null || value === undefined || (Number(value) >= 0 && Number(value) <= 3000)",
"message": "CUSTOMER_MODIFICATION.VALIDATORS.TRANSFER_LIMIT_RANGE"
}
}
]
}
},
{
"id": "customerModificationNotificationsEnabled",
"key": "notificationsEnabled",
"defaultValue": {
"notificationsEnabled": false
},
"type": "switch-group",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"options": [
{
"value": "notificationsEnabled",
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.NOTIFICATIONS.LABEL",
"checked": false
}
]
}
},
{
"id": "customerModificationPreferredContactMethod",
"key": "preferredContactMethod",
"defaultValue": "EMAIL",
"type": "button-toggle",
"className": "col-xs-12 col-sm-6",
"templateOptions": {
"required": true,
"displayColumn": true,
"variant": "radio-button",
"label": "CUSTOMER_MODIFICATION.FORM.FIELDS.PREFERRED_CONTACT.LABEL"
},
"expressionProperties": {
"templateOptions_options": "formState.selectOptionsData.preferredContactMethodOptions"
}
}
]
},
{
"fieldGroup": [
{
"id": "customerModificationSummaryHeader",
"type": "title",
"className": "d-block mt-1",
"templateOptions": {
"variant": "headline",
"size": "small",
"textSlot": "CUSTOMER_MODIFICATION.SUMMARY.TITLE"
}
},
{
"id": "customerModificationSummary",
"key": "summary",
"type": "customer-modification-summary"
}
]
}
]
}
],
"optionsData": {
"accountTypeOptions": [
{
"value": "PayrollAccount",
"label": "CUSTOMER_MODIFICATION.OPTIONS.ACCOUNT_TYPE.PAYROLL"
},
{
"value": "SavingsAccount",
"label": "CUSTOMER_MODIFICATION.OPTIONS.ACCOUNT_TYPE.SAVINGS"
},
{
"value": "BusinessAccount",
"label": "CUSTOMER_MODIFICATION.OPTIONS.ACCOUNT_TYPE.BUSINESS"
},
{
"value": "PremiumAccount",
"label": "CUSTOMER_MODIFICATION.OPTIONS.ACCOUNT_TYPE.PREMIUM"
}
],
"branchOfficeOptions": [
{
"value": "Madrid Centro",
"label": "Madrid Centro"
},
{
"value": "Barcelona Norte",
"label": "Barcelona Norte"
},
{
"value": "Sevilla Este",
"label": "Sevilla Este"
},
{
"value": "Valencia Central",
"label": "Valencia Central"
}
],
"yesNoOptions": [
{
"value": true,
"label": "OPTIONS_DATA.YES_NO.YES.LABEL"
},
{
"value": false,
"label": "OPTIONS_DATA.YES_NO.NO.LABEL"
}
],
"preferredContactMethodOptions": [
{
"value": "EMAIL",
"label": "CUSTOMER_MODIFICATION.OPTIONS.PREFERRED_CONTACT.EMAIL"
},
{
"value": "PHONE",
"label": "CUSTOMER_MODIFICATION.OPTIONS.PREFERRED_CONTACT.PHONE"
},
{
"value": "SMS",
"label": "CUSTOMER_MODIFICATION.OPTIONS.PREFERRED_CONTACT.SMS"
}
]
}
}
},
"homeIdentification": {
"form": {
"fields": [
diff --git a/src/app/app-routing.module.ts b/src/app/app-routing.module.ts
index 120431ed..39853f60 100644
--- a/src/app/app-routing.module.ts
+++ b/src/app/app-routing.module.ts
@@ -121,6 +121,14 @@ const routes: Routes = [
),
canActivate: [authGuard],
},
{
path: 'customer-modification',
loadChildren: () =>
import('./features/customer-modification/customer-modification.module').then(
(m) => m.CustomerModificationModule
),
canActivate: [authGuard],
},
{
path: ErrorService.routeError,
component: ErrorScreenComponent
diff --git a/src/app/app.module.ts b/src/app/app.module.ts
index 0d97d5c7..d564dbb2 100644
--- a/src/app/app.module.ts
+++ b/src/app/app.module.ts
@@ -51,6 +51,8 @@ import { ExcludeFieldComponent } from './shared/wrappers/exclude-components/excl
import { FormlyFieldNovationSelectMortgageRadioButtonComponent } from './shared/types/novation-select-mortgage-radio-button/novation-select-mortgage-radio-button.component';
import { FormlyFieldNovationParticipantsMultiCheckboxComponent } from './shared/types/novation-participants-multicheckbox/novation-participants-multicheckbox.component';
import { FormlyFieldNovationSummaryParticipantsInfoComponent } from './shared/types/novation-summary-participants-info/novation-summary-participants-info.component';
+import { CustomerSelectionComponent } from './features/customer-modification/components/customer-selection/customer-selection.component';
+import { CustomerModificationSummaryComponent } from './features/customer-modification/components/customer-modification-summary/customer-modification-summary.component';
// XXX: register locale data for pipes and diferent languages
registerLocaleData(localeCaES, 'ca');
@@ -213,6 +215,14 @@ export class AppModule implements DoBootstrap {
name: 'novation-summary-participants-info',
component: FormlyFieldNovationSummaryParticipantsInfoComponent,
},

{
name: 'customer-selection-radio',
component: CustomerSelectionComponent,
},
{
name: 'customer-modification-summary',
component: CustomerModificationSummaryComponent,
},
],
});
}
diff --git a/src/app/core/stubs/customer-modification.service.stub.ts b/src/app/core/stubs/customer-modification.service.stub.ts
new file mode 100644
index 00000000..90421d2c
--- /dev/null
+++ b/src/app/core/stubs/customer-modification.service.stub.ts
@@ -0,0 +1,27 @@
+import { Observable, of } from 'rxjs';
+import { CUSTOMER_MODIFICATION_CLIENTS_MOCK } from '../../../mocks';
+
+export const customerModificationServiceStub = {
getData$: (): Observable<any> =>
of([
{},
{},
{},
{
form: {
fields: [],
optionsData: {},
},
},
{},
]),
getFormConfiguration: (): Observable<any> =>
of({
form: {
fields: [],
optionsData: {},
},
}),
getClients$: (): Observable<any> => of(CUSTOMER_MODIFICATION_CLIENTS_MOCK),
getInProgressCustomer$: (_customerId: string): Observable<any> => of({ client: null }),
+};
diff --git a/src/app/core/stubs/modal-services.stub.ts b/src/app/core/stubs/modal-services.stub.ts
index 11544f74..92d6f6dd 100644
--- a/src/app/core/stubs/modal-services.stub.ts
+++ b/src/app/core/stubs/modal-services.stub.ts
@@ -3,5 +3,6 @@ import { of } from 'rxjs';
export const modalServiceStub = {
showModal: (): any => of({}),
showModalCustom: (): any => of(true),
confirm: (): any => of({})
confirm: (): any => of({}),
close: (): any => null,
};
diff --git a/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.html b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.html
new file mode 100644
index 00000000..41dc79af
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.html
@@ -0,0 +1,32 @@
+<div class="summary-container">
<homeur-box-message type="information" [showicon]="true">
<p class="summary-disclaimer">
{{ 'CUSTOMER_MODIFICATION.SUMMARY.DISCLAIMER' | translate }}
</p>
</homeur-box-message>
+
<h3 class="summary-title">
{{ 'CUSTOMER_MODIFICATION.SUMMARY.TITLE_COMPARATOR' | translate }}
</h3>
+
@if (hasChanges) {
<div class="summary-changes">
@for (change of changes; track change.fieldKey) {
<div class="change-row">
<span class="change-label">
{{ change.label | translate }}
</span>
<div class="change-values">
<span class="change-old">{{ formatValue(change.fieldKey, change.oldValue) }}</span>
<span class="change-arrow" aria-hidden="true">👉</span>
<span class="change-new">{{ formatValue(change.fieldKey, change.newValue) }}</span>
</div>
</div>
}
</div>
} @else {
<p class="no-changes-text">
{{ 'CUSTOMER_MODIFICATION.SUMMARY.NO_CHANGES' | translate }}
</p>
}
+</div>
diff --git a/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.scss b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.scss
new file mode 100644
index 00000000..2271b200
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.scss
@@ -0,0 +1,85 @@
+:host {
display: block;
+}
+
+.summary-container {
display: flex;
flex-direction: column;
gap: 24px;
margin-top: 8px;
+}
+
+.summary-disclaimer {
margin: 0;
font-size: 13px;
+}
+
+.summary-title {
font-size: 18px;
font-weight: 700;
color: var(--color-text-primary, #1a1a1a);
margin: 0;
+}
+
+.summary-changes {
display: flex;
flex-direction: column;
gap: 0;
border: 1px solid var(--color-border-default, #d9d9d9);
border-radius: 8px;
overflow: hidden;
+}
+
+.change-row {
display: flex;
justify-content: space-between;
align-items: center;
padding: 14px 16px;
gap: 16px;
border-bottom: 1px solid var(--color-border-default, #efefef);
+
&:last-child {
border-bottom: none;
}
+}
+
+.change-label {
font-size: 13px;
font-weight: 600;
color: var(--color-text-secondary, #666);
flex-shrink: 0;
min-width: 150px;
+}
+
+.change-values {
display: flex;
align-items: center;
gap: 8px;
flex-wrap: wrap;
justify-content: flex-end;
+}
+
+.change-old {
font-size: 13px;
color: var(--color-text-secondary, #666);
text-decoration: line-through;
+}
+
+.change-arrow {
font-size: 14px;
flex-shrink: 0;
+}
+
+.change-new {
font-size: 13px;
font-weight: 600;
color: var(--color-text-primary, #1a1a1a);
+}
+
+.no-changes-text {
font-size: 14px;
color: var(--color-text-secondary, #666);
text-align: center;
padding: 24px;
margin: 0;
+}
diff --git a/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.spec.ts b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.spec.ts
new file mode 100644
index 00000000..787a0503
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.spec.ts
@@ -0,0 +1,130 @@
+import { ComponentFixture, TestBed } from '@angular/core/testing';
+import { CUSTOM_ELEMENTS_SCHEMA, NO_ERRORS_SCHEMA } from '@angular/core';
+import { Pipe, PipeTransform } from '@angular/core';
+import { TranslateService } from '@ngx-translate/core';
+import { translateServiceStub } from '../../../../core/stubs/translateService.stub';
+import { CustomerModificationSummaryComponent } from './customer-modification-summary.component';
+
+@Pipe({
name: 'translate',
standalone: false,
+})
+class TranslatePipeMock implements PipeTransform {
transform(value: string): string {
return value;
}
+}
+
+describe('CustomerModificationSummaryComponent', () => {
let component: CustomerModificationSummaryComponent;
let fixture: ComponentFixture<CustomerModificationSummaryComponent>;
let translateService: TranslateService;
+
beforeEach(async () => {
await TestBed.configureTestingModule({
declarations: [CustomerModificationSummaryComponent, TranslatePipeMock],
schemas: [NO_ERRORS_SCHEMA, CUSTOM_ELEMENTS_SCHEMA],
providers: [{ provide: TranslateService, useValue: translateServiceStub }],
}).compileComponents();
+
fixture = TestBed.createComponent(CustomerModificationSummaryComponent);
component = fixture.componentInstance;
translateService = TestBed.inject(TranslateService);
});
+
it('should create', () => {
expect(component).toBeTruthy();
});
+
it('should return the changes from formState', () => {
component.field = {
options: {
formState: {
changes: [{ fieldKey: 'email', label: 'Email', oldValue: 'a', newValue: 'b' }],
},
},
} as any;
+
expect(component.changes.length).toBe(1);
});
+
it('should indicate when there are changes to display', () => {
component.field = {
options: {
formState: {
changes: [{ fieldKey: 'email', label: 'Email', oldValue: 'a', newValue: 'b' }],
},
},
} as any;
+
expect(component.hasChanges).toBeTrue();
});
+
it('should indicate when there are no changes to display', () => {
component.field = {
options: {
formState: {
changes: [],
},
},
} as any;
+
expect(component.hasChanges).toBeFalse();
});
+
it('should format null values as an em dash placeholder', () => {
component.field = {
options: {
formState: {
selectOptionsData: {},
},
},
} as any;
+
expect(component.formatValue('email', null)).toBe('—');
});
+
it('should format boolean values using translated yes no labels', () => {
component.field = {
options: {
formState: {
selectOptionsData: {},
},
},
} as any;
spyOn(translateService, 'instant').and.callFake((key: string) => {
return key === 'OPTIONS_DATA.YES_NO.YES.LABEL' ? 'Sí' : 'No';
});
+
expect(component.formatValue('notificationsEnabled', true)).toBe('Sí');
expect(component.formatValue('notificationsEnabled', false)).toBe('No');
});
+
it('should format values from select options when a matching option exists', () => {
component.field = {
options: {
formState: {
selectOptionsData: {
branchOfficeOptions: [{ value: 'MAD', label: 'Madrid' }],
},
},
},
} as any;
+
expect(component.formatValue('branchOffice', 'MAD')).toBe('Madrid');
});
+
it('should fall back to the raw value when no option match exists', () => {
component.field = {
options: {
formState: {
selectOptionsData: {
branchOfficeOptions: [{ value: 'MAD', label: 'Madrid' }],
},
},
},
} as any;
+
expect(component.formatValue('branchOffice', 'BCN')).toBe('BCN');
});
+});
diff --git a/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.ts b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.ts
new file mode 100644
index 00000000..5ddb8c14
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification-summary/customer-modification-summary.component.ts
@@ -0,0 +1,78 @@
+import { Component, inject } from '@angular/core';
+import { FieldType, FieldTypeConfig } from '@ngx-formly/core';
+import { TranslateService } from '@ngx-translate/core';
+import { CustomerModificationChange } from '../../../../shared/models/api/common/customer-modification-change.model';
+
+/**
CustomerModificationSummaryComponent
*/
+@Component({
standalone: false,
selector: 'homeur-customer-modification-summary',
templateUrl: './customer-modification-summary.component.html',
styleUrls: ['./customer-modification-summary.component.scss'],
+})
+
+export class CustomerModificationSummaryComponent extends FieldType<FieldTypeConfig> {
private readonly _translateService = inject(TranslateService);
+
/**
Returns the list of changes to be displayed in the summary, retrieved from formState.
*/
get changes(): CustomerModificationChange[] {
return this.options?.formState?.changes ?? [];
}
+
/**
Indicates whether there are any changes to be displayed in the summary.
*/
get hasChanges(): boolean {
return this.changes.length > 0;
}
/**
Formats the value of a field for display in the summary.
@param fieldKey The key of the field.
@param value The value to be formatted.
@returns The formatted value as a string.
*/
formatValue(fieldKey: string, value: any): string {
if (value === null || value === undefined) {
return '—';
}
+
if (fieldKey === 'accountType' && typeof value === 'object') {
value = value?.value ?? value?.label;
}
+
if (typeof value === 'boolean') {
return value
? this._translateService.instant('OPTIONS_DATA.YES_NO.YES.LABEL')
: this._translateService.instant('OPTIONS_DATA.YES_NO.NO.LABEL');
}
+
const optionsKey = this._getOptionsKey(fieldKey);
const options = optionsKey ? this.options?.formState?.selectOptionsData?.[optionsKey] ?? [] : [];
const translatedOption = options.find((option: any) => option.value === value);
+
if (translatedOption?.label) {
return translatedOption.label;
}
+
return String(value);
}
+
/**
Returns the key for the options corresponding to a given field key.
@param fieldKey The key of the field.
@returns The key for the options or null if not found.
*/
private _getOptionsKey(fieldKey: string): string | null {
const optionsKeys: Record<string, string> = {
accountType: 'accountTypeOptions',
branchOffice: 'branchOfficeOptions',
preferredContactMethod: 'preferredContactMethodOptions',
};
+
return optionsKeys[fieldKey] ?? null;
}
+}
diff --git a/src/app/features/customer-modification/components/customer-modification.component.html b/src/app/features/customer-modification/components/customer-modification.component.html
new file mode 100644
index 00000000..eace4442
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification.component.html
@@ -0,0 +1,18 @@
+@if (form && isDataReady) {
<div class="page-container">
<div class="page-main" [ngClass]="{ 'page-min-height': isAppIntoIFrame }">
<form
class="form-content"
[formGroup]="form"
(ngSubmit)="submit()"
<formly-form
[form]="form"
[fields]="fields"
[options]="options"
[model]="model"
</formly-form>

</form>
</div>
</div>
+}
diff --git a/src/app/features/customer-modification/components/customer-modification.component.scss b/src/app/features/customer-modification/components/customer-modification.component.scss
new file mode 100644
index 00000000..1ec22264
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification.component.scss
@@ -0,0 +1,19 @@
+:host {
display: block;
+}
+
+.page-container {
width: 100%;
+}
+
+.page-main {
width: 100%;
+
&.page-min-height {
min-height: 100vh;
}
+}
+
+.form-content {
width: 100%;
+}
diff --git a/src/app/features/customer-modification/components/customer-modification.component.spec.ts b/src/app/features/customer-modification/components/customer-modification.component.spec.ts
new file mode 100644
index 00000000..a04bbb29
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification.component.spec.ts
@@ -0,0 +1,305 @@
+import { ComponentFixture, TestBed } from '@angular/core/testing';
+import { CUSTOM_ELEMENTS_SCHEMA, NO_ERRORS_SCHEMA } from '@angular/core';
+import { FormGroup, ReactiveFormsModule } from '@angular/forms';
+import { provideRouter, Router } from '@angular/router';
+import {
ButtonControllerService,
CommunicationService,
EventsControllerService,
ModalService,
StepperService,
WindowRef,
+} from '@sanes-hipdig/lf-ng-50084125-front-compones';
+import { of } from 'rxjs';
+import { buttonControllerServiceStub } from '../../../core/stubs/buttonControllerService.stub';
+import { communicationServiceStub } from '../../../core/stubs/communication-service.stub';
+import { customerModificationServiceStub } from '../../../core/stubs/customer-modification.service.stub';
+import { eventControllerServiceStub } from '../../../core/stubs/eventControllerService.stub';
+import { modalServiceStub } from '../../../core/stubs/modal-services.stub';
+import { stepperServiceStub } from '../../../core/stubs/stepperServiceStub';
+import { tealiumDataServiceStub } from '../../../core/stubs/tealiumDataService.stub';
+import { translateOptionsServiceStub } from '../../../core/stubs/translate-options.service.stub';
+import { windowRefStub } from '../../../core/stubs/windowRef.stub';
+import { TealiumDataService } from '../../../core/services/metrics/tealium-data.service';
+import { CustomerModificationClient } from '../../../shared/models/api/common/customer-modification.model';
+import { TranslateOptionsService } from '../../../shared/services/translate-options.service';
+import { CustomerModificationService } from '../services/customer-modification.service';
+import { CustomerModificationComponent } from './customer-modification.component';
+import { ModalConfirmChangesComponent } from './modal-confirm-changes/modal-confirm-changes.component';
+
+describe('CustomerModificationComponent', () => {
let component: CustomerModificationComponent;
let fixture: ComponentFixture<CustomerModificationComponent>;
let customerModificationService: CustomerModificationService;
let translateOptionsService: TranslateOptionsService;
let buttonControllerService: ButtonControllerService;
let eventControllerService: EventsControllerService;
let stepperService: StepperService;
let modalService: ModalService;
let communicationService: CommunicationService;
let tealiumDataService: TealiumDataService;
let router: Router;
let windowRef: WindowRef;
+
const mockClients = [
{
id: 1,
fullName: 'Ana Garcia',
document: '12345678A',
email: 'ana@test.com',
phone: '600000001',
accountNumber: 'ES123',
accountType: 'CHK',
branchOffice: 'MAD',
transferLimit: 2500,
notificationsEnabled: true,
preferredContactMethod: 'EMAIL',
},
{
id: 2,
fullName: 'Luis Perez',
document: '12345678B',
email: 'luis@test.com',
phone: '600000002',
accountNumber: 'ES456',
accountType: 'SAV',
branchOffice: 'BCN',
transferLimit: 4500,
notificationsEnabled: false,
preferredContactMethod: 'PHONE',
},
] as CustomerModificationClient[];
+
const mockFormData = {
form: {
fields: [{ key: 'selectedClientId' }],
optionsData: {
accountTypeOptions: [],
},
},
} as any;
+
const mockTranslatedOptions = {
accountTypeOptions: [{ value: 'CHK', label: 'Cuenta corriente' }],
branchOfficeOptions: [{ value: 'MAD', label: 'Madrid' }],
preferredContactMethodOptions: [{ value: 'EMAIL', label: 'Email' }],
} as any;
+
const initializeComponent = (): void => {
spyOn(customerModificationService, 'getData$').and.returnValue(
of([{ anyParameter: true }, { applicant: { applicantId: 'CUST001' } }, { source: 'router' }, mockFormData, { isCustomerModification: false }])
);
spyOn(customerModificationService, 'getClients$').and.returnValue(of(mockClients));
spyOn(customerModificationService, 'getInProgressCustomer$').and.returnValue(of({ client: null }));
spyOn(translateOptionsService, 'translateOptions').and.returnValue(mockTranslatedOptions);
spyOn(buttonControllerService, 'event$').and.returnValue(of('loadSummaryStep'));
spyOn(eventControllerService, 'setEventMap');
spyOn(buttonControllerService, 'executeFunctionByName');
spyOn(stepperService, 'back');
spyOn(modalService, 'showModalCustom').and.returnValue(of({ isAccept: false }));
spyOn(communicationService, 'setBreadcrumb');
spyOn(tealiumDataService, 'executeTealium');
spyOn(router, 'navigate').and.resolveTo(true);
spyOn(windowRef, 'isAppIntoIFrame').and.returnValue(true);
+
component.ngOnInit();
};
+
beforeEach(async () => {
await TestBed.configureTestingModule({
declarations: [CustomerModificationComponent],
imports: [ReactiveFormsModule],
schemas: [NO_ERRORS_SCHEMA, CUSTOM_ELEMENTS_SCHEMA],
providers: [
provideRouter([]),
{ provide: CustomerModificationService, useValue: customerModificationServiceStub },
{ provide: TranslateOptionsService, useValue: translateOptionsServiceStub },
{ provide: ButtonControllerService, useValue: buttonControllerServiceStub },
{ provide: EventsControllerService, useValue: eventControllerServiceStub },
{ provide: StepperService, useValue: stepperServiceStub },
{ provide: ModalService, useValue: modalServiceStub },
{ provide: CommunicationService, useValue: communicationServiceStub },
{ provide: TealiumDataService, useValue: tealiumDataServiceStub },
{ provide: WindowRef, useValue: windowRefStub },
],
}).compileComponents();
+
fixture = TestBed.createComponent(CustomerModificationComponent);
component = fixture.componentInstance;
customerModificationService = TestBed.inject(CustomerModificationService);
translateOptionsService = TestBed.inject(TranslateOptionsService);
buttonControllerService = TestBed.inject(ButtonControllerService);
eventControllerService = TestBed.inject(EventsControllerService);
stepperService = TestBed.inject(StepperService);
modalService = TestBed.inject(ModalService);
communicationService = TestBed.inject(CommunicationService);
tealiumDataService = TestBed.inject(TealiumDataService);
router = TestBed.inject(Router);
windowRef = TestBed.inject(WindowRef);
});
+
it('should create', () => {
expect(component).toBeTruthy();
});
+
describe('Initialization', () => {
it('should initialize data and subscribe button events on ngOnInit', () => {
initializeComponent();
+
expect(component.form).toBeInstanceOf(FormGroup);
expect(component.fields).toEqual(mockFormData.form.fields);
expect(component.options.formState.clients).toEqual(mockClients);
expect(component.options.formState.selectOptionsData).toEqual(mockTranslatedOptions);
expect(component.isAppIntoIFrame).toBeTrue();
expect(component.isDataReady).toBeTrue();
expect(eventControllerService.setEventMap).toHaveBeenCalledWith('loadModificationFormStep', 'loadModificationFormStep');
expect(eventControllerService.setEventMap).toHaveBeenCalledWith('loadSummaryStep', 'loadSummaryStep');
expect(eventControllerService.setEventMap).toHaveBeenCalledWith('cancelCustomerModification', 'cancelCustomerModification');
expect(communicationService.setBreadcrumb).toHaveBeenCalled();
expect(buttonControllerService.executeFunctionByName).toHaveBeenCalledWith('loadSummaryStep', component);
expect(tealiumDataService.executeTealium).toHaveBeenCalledWith('customerModification.views', 'selectClient');
});
+
it('should complete the unsubscribe subject on ngOnDestroy', () => {
const nextSpy = spyOn((component as any)._unsubscribe, 'next');
const completeSpy = spyOn((component as any)._unsubscribe, 'complete');
+
component.ngOnDestroy();
+
expect(nextSpy).toHaveBeenCalled();
expect(completeSpy).toHaveBeenCalled();
});
});
+
describe('Step navigation', () => {
it('should auto-select in-progress client and move to step 2 when customer modification is in progress', () => {
spyOn(customerModificationService, 'getData$').and.returnValue(
of([{ anyParameter: true }, { applicant: { applicantId: 'CUST001' } }, { source: 'router' }, mockFormData, { isCustomerModification: true }])
);
spyOn(customerModificationService, 'getClients$').and.returnValue(of(mockClients));
spyOn(customerModificationService, 'getInProgressCustomer$').and.returnValue(of({ client: mockClients[0] }));
spyOn(translateOptionsService, 'translateOptions').and.returnValue(mockTranslatedOptions);
spyOn(buttonControllerService, 'event$').and.returnValue(of('loadSummaryStep'));
spyOn(eventControllerService, 'setEventMap');
spyOn(buttonControllerService, 'executeFunctionByName');
spyOn(stepperService, 'back');
spyOn(modalService, 'showModalCustom').and.returnValue(of({ isAccept: false }));
spyOn(communicationService, 'setBreadcrumb');
spyOn(tealiumDataService, 'executeTealium');
spyOn(router, 'navigate').and.resolveTo(true);
spyOn(windowRef, 'isAppIntoIFrame').and.returnValue(true);
+
component.ngOnInit();
+
expect(customerModificationService.getInProgressCustomer$).toHaveBeenCalledWith('CUST001');
expect(component.model.selectedClientId).toBe(1);
expect(component.model.fullName).toBe('Ana Garcia');
expect(stepperService.back).toHaveBeenCalledWith(1);
});
+
it('should not move to step 2 when customer modification is in progress but no in-progress client exists', () => {
spyOn(customerModificationService, 'getData$').and.returnValue(
of([{ anyParameter: true }, { applicant: { applicantId: 'CUST001' } }, { source: 'router' }, mockFormData, { isCustomerModification: true }])
);
spyOn(customerModificationService, 'getClients$').and.returnValue(of(mockClients));
spyOn(customerModificationService, 'getInProgressCustomer$').and.returnValue(of({ client: null }));
spyOn(translateOptionsService, 'translateOptions').and.returnValue(mockTranslatedOptions);
spyOn(buttonControllerService, 'event$').and.returnValue(of('loadSummaryStep'));
spyOn(eventControllerService, 'setEventMap');
spyOn(buttonControllerService, 'executeFunctionByName');
spyOn(stepperService, 'back');
spyOn(modalService, 'showModalCustom').and.returnValue(of({ isAccept: false }));
spyOn(communicationService, 'setBreadcrumb');
spyOn(tealiumDataService, 'executeTealium');
spyOn(router, 'navigate').and.resolveTo(true);
spyOn(windowRef, 'isAppIntoIFrame').and.returnValue(true);
+
component.ngOnInit();
+
expect(component.model.selectedClientId).toBeUndefined();
expect(stepperService.back).not.toHaveBeenCalledWith(1);
});
+
it('should load selected client data into the form and move to step 2', () => {
initializeComponent();
component.model = { selectedClientId: 1 };
+
component.loadModificationFormStep();
+
expect(component.model.fullName).toBe('Ana Garcia');
expect(component.model.email).toBe('ana@test.com');
expect(component.model.preferredContactMethod).toBe('EMAIL');
expect(stepperService.back).toHaveBeenCalledWith(1);
expect(tealiumDataService.executeTealium).toHaveBeenCalledWith('customerModification.views', 'modifyClient');
});
+
it('should not move to step 2 when no client is selected', () => {
initializeComponent();
component.model = { selectedClientId: 999 };
+
component.loadModificationFormStep();
+
expect(stepperService.back).not.toHaveBeenCalledWith(1);
});
+
it('should calculate changes and move to summary step', () => {
initializeComponent();
component.model = { selectedClientId: 1 };
component.loadModificationFormStep();
(stepperService.back as jasmine.Spy).calls.reset();
component.model.email = 'updated@test.com';
component.model.notificationsEnabled = false;
+
component.loadSummaryStep();
+
expect(component.options.formState.changes.length).toBe(2);
expect(component.options.formState.changes).toContain(
jasmine.objectContaining({
fieldKey: 'email',
oldValue: 'ana@test.com',
newValue: 'updated@test.com',
})
);
expect(component.options.formState.changes).toContain(
jasmine.objectContaining({
fieldKey: 'notificationsEnabled',
oldValue: true,
newValue: false,
})
);
expect(stepperService.back).toHaveBeenCalledWith(2);
expect(tealiumDataService.executeTealium).toHaveBeenCalledWith('customerModification.views', 'summary');
});
});
+
describe('Actions', () => {
it('should navigate to distributor when cancellation is requested', () => {
spyOn(router, 'navigate').and.resolveTo(true);
+
component.cancelCustomerModification();
+
expect(router.navigate).toHaveBeenCalledWith(['/distributor']);
});
+
it('should open confirmation modal on submit', () => {
initializeComponent();
+
component.submit();
+
expect(modalService.showModalCustom).toHaveBeenCalledWith(ModalConfirmChangesComponent, {
modalSize: 'medium',
data: {},
});
});
+
it('should navigate after accepting the confirmation modal', () => {
initializeComponent();
(modalService.showModalCustom as jasmine.Spy).and.returnValue(of({ isAccept: true }));
+
component.submit();
+
expect(tealiumDataService.executeTealium).toHaveBeenCalledWith('customerModification.events', 'modificationConfirmed');
expect(router.navigate).toHaveBeenCalledWith(['/distributor']);
});
});
+});
diff --git a/src/app/features/customer-modification/components/customer-modification.component.ts b/src/app/features/customer-modification/components/customer-modification.component.ts
new file mode 100644
index 00000000..b71b31ab
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-modification.component.ts
@@ -0,0 +1,349 @@
+/ eslint-disable @typescript-eslint/naming-convention /
+import { Component, OnDestroy, OnInit, inject } from '@angular/core';
+import { FormGroup } from '@angular/forms';
+import { Router } from '@angular/router';
+import { FormlyFieldConfig, FormlyFormOptions } from '@ngx-formly/core';
+import {
ButtonControllerService,
CommunicationService,
EventsControllerService,
ModalService,
StepperService,
WindowRef,
+} from '@sanes-hipdig/lf-ng-50084125-front-compones';
+import { Subject, take, takeUntil } from 'rxjs';
+import { TealiumDataService } from '../../../core/services/metrics/tealium-data.service';
+import {
CustomerModificationChange,
CustomerModificationOption,
+} from '../../../shared/models/api/common/customer-modification-change.model';
+import { CustomerModificationClient } from '../../../shared/models/api/common/customer-modification.model';
+import { TranslateOptionsService } from '../../../shared/services/translate-options.service';
+import { ModalConfirmChangesComponent } from './modal-confirm-changes/modal-confirm-changes.component';
+import { CustomerModificationService } from '../services/customer-modification.service';
+/**
CustomerModificationComponent
*/
+@Component({
standalone: false,
selector: 'homeur-customer-modification',
templateUrl: './customer-modification.component.html',
styleUrls: ['./customer-modification.component.scss'],
+})
+export class CustomerModificationComponent implements OnInit, OnDestroy {
private readonly _windowRef = inject(WindowRef);
private readonly _customerModificationService = inject(CustomerModificationService);
private readonly _translateOptionsService = inject(TranslateOptionsService);
private readonly _buttonControllerService = inject(ButtonControllerService);
private readonly _eventController = inject(EventsControllerService);
private readonly _stepperService = inject(StepperService);
private readonly _modalService = inject(ModalService);
private readonly _communicationService = inject(CommunicationService);
private readonly _tealiumDataService = inject(TealiumDataService);
private readonly _router = inject(Router);
+
private readonly _unsubscribe: Subject<void> = new Subject();
+
private _originalClientData: CustomerModificationClient | null = null;
private _clients: CustomerModificationClient[] = [];
private _translatedOptionsData: Record<string, CustomerModificationOption[]> = {};
private _isCustomerModificationInProgress = false;
private _inProgressAutoSelectApplied = false;
private _customerId: string | undefined;
+
form!: FormGroup;
model: any = {};
fields: FormlyFieldConfig[] = [];
options!: FormlyFormOptions;
isAppIntoIFrame!: boolean;
isDataReady = false;
+
/**
OnInit
*/
ngOnInit(): void {
this._eventController.setEventMap('loadModificationFormStep', 'loadModificationFormStep');
this._eventController.setEventMap('loadSummaryStep', 'loadSummaryStep');
this._eventController.setEventMap('cancelCustomerModification', 'cancelCustomerModification');
+
this._initializeDataSurvey();
this._subscribeToButtonController();
this._tealiumDataService.executeTealium('customerModification.views', 'selectClient');
}
+
/**
OnDestroy
*/
ngOnDestroy(): void {
this._unsubscribe.next();
this._unsubscribe.complete();
}
+
/**
Initializes data survey
*/
private _initializeDataSurvey(): void {
this.isAppIntoIFrame = this._windowRef.isAppIntoIFrame();
this.form = new FormGroup({});
+
this._customerModificationService
.getData$()
.pipe(takeUntil(this._unsubscribe))
.subscribe(([parameters, customer, routerParams, formData, validateExistingApplication]) => {
if (!parameters || !formData) {
return;
}
+
this._customerId = customer?.applicant?.applicantId;
this._isCustomerModificationInProgress = Boolean(validateExistingApplication?.isCustomerModification);
+
if (routerParams) {
this._communicationService.setBreadcrumb([
{ title: 'BREADCRUMB.DISTRIBUTOR_TITLE' },
{ title: 'BREADCRUMB.CUSTOMER_MODIFICATION_TITLE' },
]);
}
+
this.fields = formData.form?.fields ?? [];
this._translatedOptionsData = this._translateOptionsService.translateOptions({ ...formData.form?.optionsData });
this.options = {
formState: {
selectOptionsData: this._getSelectOptionsData(),
clients: this._clients,
changes: [],
lockSelectedClient: false,
lockedSelectedClientId: null,
},
};
this.isDataReady = true;
this._tryAutoLoadCustomerModificationStep();
});
this._loadClients();
}
+
/**
Load Clients
*/
private _loadClients(): void {
this._customerModificationService
.getClients$()
.pipe(takeUntil(this._unsubscribe))
.subscribe({
next: clients => {
this._clients = clients ?? [];
if (this._clients.length === 0) {
this._tealiumDataService.executeTealium('customerModification.events', 'noClientsAvailable');
}
if (this.options?.formState) {
this.options.formState.clients = this._clients;
this.options.formState.selectOptionsData = this._getSelectOptionsData();
this.model = { ...this.model };
}
this._tryAutoLoadCustomerModificationStep();
},
error: () => {
this._clients = [];
},
});
}
+
/**
Loads in-progress customer context and navigates to step 2 when applicable.
*/
private _tryAutoLoadCustomerModificationStep(): void {
if (this._inProgressAutoSelectApplied || !this.isDataReady || !this._customerId || this._clients.length === 0) {
return;
}
+
if (!this._isCustomerModificationInProgress) {
return;
}
+
this._inProgressAutoSelectApplied = true;
this._customerModificationService
.getInProgressCustomer$(this._customerId)
.pipe(take(1), takeUntil(this._unsubscribe))
.subscribe((inProgressContext) => {
const selectedClient = inProgressContext?.client;
+
if (!selectedClient) {
return;
}
+
if (this.options?.formState) {
this.options.formState.lockSelectedClient = true;
this.options.formState.lockedSelectedClientId = selectedClient.id;
}
this.model = {
...this.model,
selectedClientId: selectedClient.id,
};
this.loadModificationFormStep();
});
}
+
/**
Returns the select options data with translated labels for display in the form.
@returns The select options data with translated labels.
*/
private _getSelectOptionsData(): Record<string, CustomerModificationOption[]> {
return {
...this._translatedOptionsData,
};
}
+
/**
Normalizes client account type to the option value expected by the accountType control.
@param accountType Client account type value.
@returns Account type option value compatible with the control.
*/
private _normalizeAccountTypeValue(accountType: string): string {
const accountTypeOptions = this.options?.formState?.selectOptionsData?.accountTypeOptions ?? [];
const matchedOption = accountTypeOptions.find(
(option: CustomerModificationOption) => option.value === accountType || option.label === accountType,
);
return matchedOption?.value ?? accountType;
}
+
/**
Normalizes notificationsEnabled value from switch-group model shape to a boolean.
@param value Current model value for notificationsEnabled.
@returns Normalized boolean value.
*/
private _normalizeNotificationsEnabledValue(value: unknown): boolean {
if (typeof value === 'boolean') {
return value;
}
+
if (value && typeof value === 'object' && 'notificationsEnabled' in value) {
return Boolean((value as Record<string, unknown>).notificationsEnabled);
}
+
return false;
}
+
/**
Subscribes to button controller events and executes the corresponding functions when events are triggered.
The function names are mapped to events using the EventsControllerService.
*/
private _subscribeToButtonController(): void {
this._buttonControllerService
.event$()
.pipe(takeUntil(this._unsubscribe))
.subscribe(event => {
this._buttonControllerService.executeFunctionByName(event, this);
});
}
+
/**
Calculates the list of changes made to the client's data by comparing the original data with the current model values.
@returns An array of CustomerModificationChange objects representing the changes made.
*/
private _calculateChanges(): CustomerModificationChange[] {
if (!this._originalClientData) {
return [];
}
+
const fieldLabels: Record<string, string> = {
fullName: 'CUSTOMER_MODIFICATION.FORM.FIELDS.FULL_NAME.LABEL',
email: 'CUSTOMER_MODIFICATION.FORM.FIELDS.EMAIL.LABEL',
phone: 'CUSTOMER_MODIFICATION.FORM.FIELDS.PHONE.LABEL',
accountNumber: 'CUSTOMER_MODIFICATION.FORM.FIELDS.ACCOUNT_NUMBER.LABEL',
accountType: 'CUSTOMER_MODIFICATION.FORM.FIELDS.ACCOUNT_TYPE.LABEL',
branchOffice: 'CUSTOMER_MODIFICATION.FORM.FIELDS.BRANCH_OFFICE.LABEL',
transferLimit: 'CUSTOMER_MODIFICATION.FORM.FIELDS.TRANSFER_LIMIT.LABEL',
notificationsEnabled: 'CUSTOMER_MODIFICATION.FORM.FIELDS.NOTIFICATIONS.LABEL',
preferredContactMethod: 'CUSTOMER_MODIFICATION.FORM.FIELDS.PREFERRED_CONTACT.LABEL',
};
+
return Object.keys(fieldLabels)
.filter(key => {
const currentValue =
key === 'notificationsEnabled'
? this._normalizeNotificationsEnabledValue(this.model[key])
: this.model[key];
+
return this._originalClientData![key as keyof CustomerModificationClient] !== currentValue;
})
.map(key => ({
fieldKey: key,
label: fieldLabels[key],
oldValue: this._originalClientData![key as keyof CustomerModificationClient],
newValue:
key === 'notificationsEnabled'
? this._normalizeNotificationsEnabledValue(this.model[key])
: this.model[key],
}));
}
+
/**
Loads the modification form step for the selected client.
It populates the form with the client's current data and navigates to the modification step.
*/
loadModificationFormStep(): void {
const selectedClientId = this.model.selectedClientId as number;
const selectedClient = this._clients.find(c => c.id === selectedClientId);
+
if (!selectedClient) {
return;
}
+
const normalizedAccountType = this._normalizeAccountTypeValue(selectedClient.accountType);
+
this._originalClientData = { ...selectedClient, accountType: normalizedAccountType };
+
this.model = {
...this.model,
fullName: selectedClient.fullName,
email: selectedClient.email,
phone: selectedClient.phone,
accountNumber: selectedClient.accountNumber,
accountType: normalizedAccountType,
branchOffice: selectedClient.branchOffice,
transferLimit: selectedClient.transferLimit,
notificationsEnabled: {
notificationsEnabled: selectedClient.notificationsEnabled,
},
preferredContactMethod: selectedClient.preferredContactMethod,
};
+
this._stepperService.back(1);
this._tealiumDataService.executeTealium('customerModification.views', 'modifyClient');
}
+
/**
Loads the summary step of the modification process.
It calculates the changes made to the client's data and updates the form state.
*/
loadSummaryStep(): void {
const changes = this._calculateChanges();
this.options.formState.changes = changes;
this.model = { ...this.model };
this._stepperService.back(2);
this._tealiumDataService.executeTealium('customerModification.views', 'summary');
}
+
/**
Cancels the customer modification process and navigates back to the distributor view.
*/
cancelCustomerModification(): void {
this._router.navigate(['/distributor']);
}
+
/**
Submits the customer modification changes.
*/
submit(): void {
this._modalService
.showModalCustom(ModalConfirmChangesComponent, {
modalSize: 'medium',
data: {},
})
.subscribe(result => {
if (result?.isAccept) {
this._tealiumDataService.executeTealium('customerModification.events', 'modificationConfirmed');
this._router.navigate(['/distributor']);
}
});
}
+}
diff --git a/src/app/features/customer-modification/components/customer-selection/customer-selection.component.html b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.html
new file mode 100644
index 00000000..f7fcc5a6
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.html
@@ -0,0 +1,47 @@
+@if (clients.length > 0) {
<div class="customer-radio-group" [formlyAttributes]="field">
@for (client of clients; track client.id) {
<label
class="customer-radio-option"
[class.checked]="formControl.value === client.id"
<input
type="radio"
[name]="field.key || 'selectedClientId'"
[value]="client.id"
[checked]="formControl.value === client.id"
[disabled]="isSelectionLocked && lockedSelectedClientId !== client.id"
(change)="onSelectClient(client.id)"
/>
<div class="radio-option-content">
<div class="option-row row-header">
<span class="option-label">{{ 'CUSTOMER_MODIFICATION.FORM.FIELDS.FULL_NAME.LABEL' | translate }}</span>
<span class="option-value">{{ client.fullName }}</span>
</div>
<div class="option-row">
<span class="option-label">{{ client.document }}</span>
<span class="option-value amount">{{ getAccountTypeLabel(client.accountType) }}</span>
</div>
<div class="option-row row-amount">
<span class="option-label">{{ 'CUSTOMER_MODIFICATION.FORM.FIELDS.ACCOUNT_NUMBER.LABEL' | translate }}</span>
<span class="option-value amount">{{ client.accountNumber }}</span>
</div>
</div>
</label>
}
</div>
+} @else {
<div class="customer-selection-empty no-clients-container">
<div class="empty-image">
<img src="mf-ng-50078458-homeplanner/assets/images/icons/empty-mortgage-list.svg" alt="" />
</div>
<div class="empty-body">
<span class="empty-title">
{{ 'CUSTOMER_MODIFICATION.NO_CLIENTS' | translate }}
</span>
<span class="empty-desc">
{{ 'CUSTOMER_MODIFICATION.NO_CLIENTS_DESC' | translate }}
</span>
</div>
</div>
+}
diff --git a/src/app/features/customer-modification/components/customer-selection/customer-selection.component.scss b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.scss
new file mode 100644
index 00000000..05f958af
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.scss
@@ -0,0 +1,187 @@
+@use 'globals-vars' as *;
+
+.customer-radio-group {
display: flex;
flex-direction: column;
gap: 12px;
padding-bottom: 16px;
+
.customer-radio-option {
display: flex;
border: 1px solid #ccc;
border-radius: 8px;
padding: 12px;
margin: 0 5px 0 0;
position: relative;
cursor: pointer;
+
&:hover {
border-color: #127277;
}
+
&.checked {
border: 2px solid #127277;
background-color: rgba(19, 126, 132, 0.08);
}
+
input[type='radio'] {
margin-right: 12px;
margin-top: 4px;
accent-color: #127277;
cursor: pointer;
-webkit-appearance: none;
appearance: none;
width: 16px;
height: 16px;
min-width: 16px;
border: 1px solid #aaa;
border-radius: 50%;
position: relative;
+
&:checked {
border: 0.125rem solid #127277;
background-color: #fff;
+
&::after {
content: '';
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);
width: 8px;
height: 8px;
border-radius: 50%;
background-color: #127277;
}
}
+
&:hover {
border-color: #127277;
}
+
&:focus {
outline: none;
}
}
+
.radio-option-content {
margin-left: 4px;
width: 100%;
+
.option-row {
display: grid;
grid-template-columns: 1fr;
gap: 4px;
+
&.row-header {
border-bottom: 1px dashed #ccc;
padding-bottom: 8px;
}
+
&.row-amount {
padding-top: 8px;
}
+
.option-label {
font-family: $santander-micro-regular;
font-weight: 400;
font-size: 14px;
line-height: 24px;
color: #222222;
margin: 0;
justify-self: start;
}
+
.option-value {
font-family: $santander-headline-bold;
font-weight: 700;
font-size: 16px;
line-height: 24px;
color: #222222;
text-align: left;
justify-self: start;
+
&.amount {
font-family: $santander-headline-regular;
font-weight: 400;
font-size: 14px;
line-height: 24px;
}
}
}
}
}
+}
+
+.no-clients-container {
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
+
.empty-body {
display: flex;
flex-direction: column;
align-items: center;
justify-content: center;
gap: 8px;
padding: 20px 0 32px 0;
+
.empty-title {
font-family: $santander-headline-bold;
font-weight: 700;
font-size: 24px;
line-height: 32px;
color: #222222;
text-align: center;
margin: 0;
}
+
.empty-desc {
font-family: $santander-headline-regular;
font-size: 16px;
line-height: 24px;
color: #222222;
text-align: center;
margin: 0;
}
}
+}
+
+@media screen and (min-width: $bp-m) {
.customer-radio-group {
gap: 14px;
+
.customer-radio-option {
padding: 16px;
margin: 0 5px 0 0;
+
.radio-option-content .option-row {
grid-template-columns: 1fr 1fr;
gap: 16px;
+
&:first-child {
padding-bottom: 12px;
}
+
&:last-child {
padding-top: 12px;
}
+
.option-label {
font-size: 16px;
}
+
.option-value {
font-size: 20px;
text-align: end;
justify-self: end;
+
&.amount {
font-size: 18px;
}
}
}
}
}
+}
diff --git a/src/app/features/customer-modification/components/customer-selection/customer-selection.component.spec.ts b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.spec.ts
new file mode 100644
index 00000000..e0296061
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.spec.ts
@@ -0,0 +1,150 @@
+import { ComponentFixture, TestBed } from '@angular/core/testing';
+import { CUSTOM_ELEMENTS_SCHEMA, NO_ERRORS_SCHEMA } from '@angular/core';
+import { FormControl } from '@angular/forms';
+import { CustomerModificationClient } from '../../../../shared/models/api/common/customer-modification.model';
+import { CustomerSelectionComponent } from './customer-selection.component';
+
+describe('CustomerSelectionComponent', () => {
let component: CustomerSelectionComponent;
let fixture: ComponentFixture<CustomerSelectionComponent>;
+
const mockClients = [
{
id: 1,
fullName: 'Ana Garcia',
document: '12345678A',
email: 'ana@test.com',
phone: '600000001',
accountNumber: 'ES123',
accountType: 'CHK',
branchOffice: 'MAD',
transferLimit: 2500,
notificationsEnabled: true,
preferredContactMethod: 'EMAIL',
},
] as CustomerModificationClient[];
+
beforeEach(async () => {
await TestBed.configureTestingModule({
declarations: [CustomerSelectionComponent],
schemas: [NO_ERRORS_SCHEMA, CUSTOM_ELEMENTS_SCHEMA],
}).compileComponents();
+
fixture = TestBed.createComponent(CustomerSelectionComponent);
component = fixture.componentInstance;
});
+
it('should create', () => {
expect(component).toBeTruthy();
});
+
it('should return clients from formState', () => {
component.field = {
options: {
formState: {
clients: mockClients,
},
},
formControl: new FormControl(null),
} as any;
+
expect(component.clients).toEqual(mockClients);
});
+
it('should return an empty clients list when formState is missing', () => {
component.field = {
options: {},
formControl: new FormControl(null),
} as any;
+
expect(component.clients).toEqual([]);
});
+
it('should resolve the translated account type label', () => {
component.field = {
options: {
formState: {
selectOptionsData: {
accountTypeOptions: [{ value: 'CHK', label: 'Cuenta corriente' }],
},
},
},
formControl: new FormControl(null),
} as any;
+
expect(component.getAccountTypeLabel('CHK')).toBe('Cuenta corriente');
});
+
it('should fall back to the raw account type when there is no option match', () => {
component.field = {
options: {
formState: {
selectOptionsData: {
accountTypeOptions: [{ value: 'CHK', label: 'Cuenta corriente' }],
},
},
},
formControl: new FormControl(null),
} as any;
+
expect(component.getAccountTypeLabel('SAV')).toBe('SAV');
});
+
it('should set the selected client into the form control', () => {
const mockFormControl = new FormControl(null);
spyOn(mockFormControl, 'markAsTouched');
component.field = {
options: {
formState: {
clients: mockClients,
},
},
formControl: mockFormControl,
} as any;
+
component.onSelectClient(1);
+
expect(component.formControl.value).toBe(1);
expect(mockFormControl.markAsTouched).toHaveBeenCalled();
});
+
it('should not allow changing selection when selection is locked', () => {
const mockFormControl = new FormControl(1);
spyOn(mockFormControl, 'markAsTouched');
component.field = {
options: {
formState: {
clients: mockClients,
lockSelectedClient: true,
lockedSelectedClientId: 1,
},
},
formControl: mockFormControl,
} as any;
+
component.onSelectClient(2);
+
expect(component.formControl.value).toBe(1);
expect(mockFormControl.markAsTouched).not.toHaveBeenCalled();
});
+
it('should allow selecting the locked client when selection is locked', () => {
const mockFormControl = new FormControl(1);
spyOn(mockFormControl, 'markAsTouched');
component.field = {
options: {
formState: {
clients: mockClients,
lockSelectedClient: true,
lockedSelectedClientId: 1,
},
},
formControl: mockFormControl,
} as any;
+
component.onSelectClient(1);
+
expect(component.formControl.value).toBe(1);
expect(mockFormControl.markAsTouched).toHaveBeenCalled();
});
+});
diff --git a/src/app/features/customer-modification/components/customer-selection/customer-selection.component.ts b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.ts
new file mode 100644
index 00000000..c1342642
--- /dev/null
+++ b/src/app/features/customer-modification/components/customer-selection/customer-selection.component.ts
@@ -0,0 +1,62 @@
+import { Component } from '@angular/core';
+import { FieldType, FieldTypeConfig } from '@ngx-formly/core';
+import { CustomerModificationClient } from '../../../../shared/models/api/common/customer-modification.model';
+
+/**
CustomerSelectionComponent
*/
+@Component({
standalone: false,
selector: 'homeur-customer-selection',
templateUrl: './customer-selection.component.html',
styleUrls: ['./customer-selection.component.scss'],
+})
+
+export class CustomerSelectionComponent extends FieldType<FieldTypeConfig> {
+
/**
Indicates if the selected client must remain locked.
*/
get isSelectionLocked(): boolean {
return Boolean(this.options?.formState?.lockSelectedClient);
}
+
/**
Returns the locked selected client id when lock mode is enabled.
*/
get lockedSelectedClientId(): number | null {
return this.options?.formState?.lockedSelectedClientId ?? null;
}
+
/**
Returns the list of clients to be displayed in the selection, retrieved from formState.
*/
get clients(): CustomerModificationClient[] {
return this.options?.formState?.clients ?? [];
}
+
/**
Returns the label for a given account type.
@param accountType The account type value.
@returns The label for the account type.
*/
getAccountTypeLabel(accountType: string): string {
const accountTypeOptions = this.options?.formState?.selectOptionsData?.accountTypeOptions ?? [];
const selectedOption = accountTypeOptions.find((option: any) => option.value === accountType);
+
return selectedOption?.label ?? accountType;
}
+
/**
Handles the selection of a client.
@param clientId The ID of the selected client.
*/
onSelectClient(clientId: number): void {
if (this.isSelectionLocked && this.lockedSelectedClientId !== null && clientId !== this.lockedSelectedClientId) {
return;
}
+
this.formControl.setValue(clientId);
this.formControl.markAsTouched();
}
+}
diff --git a/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.html b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.html
new file mode 100644
index 00000000..0272d939
--- /dev/null
+++ b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.html
@@ -0,0 +1,22 @@
+<div class="modal-container">
<div class="modal-icon">
<lib-icon
iconcontent="correct-circle"
size="size-48"
</lib-icon>

</div>
+
<h2 class="modal-title">
{{ 'CUSTOMER_MODIFICATION.MODAL.TITLE' | translate }}
</h2>
+
<p class="modal-text">
{{ 'CUSTOMER_MODIFICATION.MODAL.TEXT' | translate }}
</p>
+
<homeur-button
homeurLabel="CUSTOMER_MODIFICATION.MODAL.ACCEPT"
[homeurClass]="'button regular-button'"
(click)="accept()"
</homeur-button>

+</div>
diff --git a/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.scss b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.scss
new file mode 100644
index 00000000..5f71d96b
--- /dev/null
+++ b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.scss
@@ -0,0 +1,26 @@
+.modal-container {
display: flex;
flex-direction: column;
align-items: center;
gap: 16px;
padding: 8px 0 4px;
text-align: center;
+}
+
+.modal-icon {
color: var(--color-success, #007a33);
+}
+
+.modal-title {
font-size: 20px;
font-weight: 700;
color: var(--color-text-primary, #1a1a1a);
margin: 0;
+}
+
+.modal-text {
font-size: 14px;
color: var(--color-text-secondary, #666);
margin: 0;
max-width: 340px;
+}
diff --git a/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.spec.ts b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.spec.ts
new file mode 100644
index 00000000..3d4b4248
--- /dev/null
+++ b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.spec.ts
@@ -0,0 +1,46 @@
+import { ComponentFixture, TestBed } from '@angular/core/testing';
+import { CUSTOM_ELEMENTS_SCHEMA, NO_ERRORS_SCHEMA } from '@angular/core';
+import { Pipe, PipeTransform } from '@angular/core';
+import { ModalService } from '@sanes-hipdig/lf-ng-50084125-front-compones';
+import { modalServiceStub } from '../../../../core/stubs/modal-services.stub';
+import { ModalConfirmChangesComponent } from './modal-confirm-changes.component';
+
+@Pipe({
name: 'translate',
standalone: false,
+})
+class TranslatePipeMock implements PipeTransform {
transform(value: string): string {
return value;
}
+}
+
+describe('ModalConfirmChangesComponent', () => {
let component: ModalConfirmChangesComponent;
let fixture: ComponentFixture<ModalConfirmChangesComponent>;
let modalService: ModalService;
+
beforeEach(async () => {
await TestBed.configureTestingModule({
declarations: [ModalConfirmChangesComponent, TranslatePipeMock],
schemas: [NO_ERRORS_SCHEMA, CUSTOM_ELEMENTS_SCHEMA],
providers: [{ provide: ModalService, useValue: modalServiceStub }],
}).compileComponents();
+
fixture = TestBed.createComponent(ModalConfirmChangesComponent);
component = fixture.componentInstance;
modalService = TestBed.inject(ModalService);
});
+
it('should create', () => {
expect(component).toBeTruthy();
});
+
it('should close the modal with accepted result', () => {
spyOn(modalService, 'close');
+
component.accept();
+
expect(modalService.close).toHaveBeenCalledWith({ isAccept: true });
});
+});
diff --git a/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.ts b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.ts
new file mode 100644
index 00000000..1bf32133
--- /dev/null
+++ b/src/app/features/customer-modification/components/modal-confirm-changes/modal-confirm-changes.component.ts
@@ -0,0 +1,24 @@
+import { Component, inject } from '@angular/core';
+import { ModalService, SanTypeIcon } from '@sanes-hipdig/lf-ng-50084125-front-compones';
+
+/**
ModalConfirmChangesComponent
*/
+@Component({
standalone: false,
selector: 'homeur-modal-confirm-changes',
templateUrl: './modal-confirm-changes.component.html',
styleUrls: ['./modal-confirm-changes.component.scss'],
+})
+export class ModalConfirmChangesComponent {
private readonly _modalService = inject(ModalService);
+
sanTypeIcon = SanTypeIcon;
+
/**
Handles the acceptance of changes.
*/
accept(): void {
this._modalService.close({ isAccept: true });
}
+}
diff --git a/src/app/features/customer-modification/customer-modification-routing.module.ts b/src/app/features/customer-modification/customer-modification-routing.module.ts
new file mode 100644
index 00000000..08479a01
--- /dev/null
+++ b/src/app/features/customer-modification/customer-modification-routing.module.ts
@@ -0,0 +1,16 @@
+import { NgModule } from '@angular/core';
+import { RouterModule, Routes } from '@angular/router';
+import { CustomerModificationComponent } from './components/customer-modification.component';
+
+const routes: Routes = [
{ path: '', component: CustomerModificationComponent }
+];
+
+/**
CustomerModificationRoutingModule
*/
+@NgModule({
imports: [RouterModule.forChild(routes)],
exports: [RouterModule],
+})
+export class CustomerModificationRoutingModule {}
diff --git a/src/app/features/customer-modification/customer-modification.module.ts b/src/app/features/customer-modification/customer-modification.module.ts
new file mode 100644
index 00000000..0a5474c8
--- /dev/null
+++ b/src/app/features/customer-modification/customer-modification.module.ts
@@ -0,0 +1,35 @@
+import { CommonModule } from '@angular/common';
+import { NgModule } from '@angular/core';
+import { TranslateModule } from '@ngx-translate/core';
+import { SharedModule } from '../../shared/shared.module';
+import { FormlySharedModule } from '../../shared/types/formly.module';
+import { FormlySharedModule as FormlySharedModuleLib } from '@sanes-hipdig/lf-ng-50084125-front-compones';
+import { SharedModule as SharedModuleLib } from '@sanes-hipdig/lf-ng-50084125-front-compones';
+
+import { CustomerModificationRoutingModule } from './customer-modification-routing.module';
+import { CustomerModificationComponent } from './components/customer-modification.component';
+import { CustomerSelectionComponent } from './components/customer-selection/customer-selection.component';
+import { CustomerModificationSummaryComponent } from './components/customer-modification-summary/customer-modification-summary.component';
+import { ModalConfirmChangesComponent } from './components/modal-confirm-changes/modal-confirm-changes.component';
+
+/**
CustomerModificationModule
*/
+@NgModule({
declarations: [
CustomerModificationComponent,
CustomerSelectionComponent,
CustomerModificationSummaryComponent,
ModalConfirmChangesComponent,
],
imports: [
CommonModule,
CustomerModificationRoutingModule,
TranslateModule,
SharedModule,
FormlySharedModule,
FormlySharedModuleLib,
SharedModuleLib,
],
+})
+export class CustomerModificationModule {}
diff --git a/src/app/features/customer-modification/services/customer-modification.service.spec.ts b/src/app/features/customer-modification/services/customer-modification.service.spec.ts
new file mode 100644
index 00000000..06f5b172
--- /dev/null
+++ b/src/app/features/customer-modification/services/customer-modification.service.spec.ts
@@ -0,0 +1,138 @@
+import { HttpTestingController, provideHttpClientTesting } from '@angular/common/http/testing';
+import { TestBed } from '@angular/core/testing';
+import { provideHttpClient } from '@angular/common/http';
+import { StorageService } from '../../../core/services';
+import { storageServiceStub } from '../../../core/stubs/StorageServiceStub';
+import { CustomerModificationClient } from '../../../shared/models/api/common/customer-modification.model';
+import { CustomerModificationService } from './customer-modification.service';
+import { UtilsApi } from '@sanes-hipdig/lf-ng-50084125-front-compones';
+import { of } from 'rxjs';
+
+describe('CustomerModificationService', () => {
let service: CustomerModificationService;
let storageService: StorageService;
let httpTestingController: HttpTestingController;
let utilsApiSpy: jasmine.SpyObj<UtilsApi>;
+
const mockFormConfiguration = {
form: {
fields: [{ key: 'selectedClientId' }],
optionsData: {},
},
} as any;
+
const mockParameters = {
mortgagesOriginationCatalogue: {
parameter: {
customerModification: mockFormConfiguration,
},
},
} as any;
+
const mockClients = [
{
id: 1,
fullName: 'Ana Garcia',
document: '12345678A',
email: 'ana@test.com',
phone: '600000001',
accountNumber: 'ES123',
accountType: 'CHK',
branchOffice: 'MAD',
transferLimit: 2500,
notificationsEnabled: true,
preferredContactMethod: 'EMAIL',
},
] as CustomerModificationClient[];
+
beforeEach(() => {
utilsApiSpy = jasmine.createSpyObj('UtilsApi', ['getEndPointUrl']);
utilsApiSpy.getEndPointUrl.and.callFake((config: any): string => {
if (config?.mocked && config?.urlMock) {
return ${config.urlMock}.json;
}
+
return config?.url ?? '';
});
+
TestBed.configureTestingModule({
providers: [
provideHttpClient(),
provideHttpClientTesting(),
CustomerModificationService,
{ provide: StorageService, useValue: storageServiceStub },
{ provide: UtilsApi, useValue: utilsApiSpy },
],
});
+
service = TestBed.inject(CustomerModificationService);
storageService = TestBed.inject(StorageService);
httpTestingController = TestBed.inject(HttpTestingController);
});
+
afterEach(() => {
httpTestingController.verify();
});
+
it('should be created', () => {
expect(service).toBeTruthy();
});
+
describe('Data Retrieval Methods', () => {
it('should combine parameters customer route params and form configuration in getData$', () => {
const mockCustomer = { applicant: { applicantId: '1' } } as any;
const mockRouteParams = { channel: 'INT' } as any;
const mockValidateExistingApp = { isCustomerModification: true } as any;
spyOn(storageService, 'getParameters').and.returnValue(of(mockParameters));
spyOn(storageService, 'getCustomer').and.returnValue(of(mockCustomer));
spyOn(storageService, 'getRouteParams').and.returnValue(of(mockRouteParams));
spyOn(storageService, 'getValidateExistingApp').and.returnValue(of(mockValidateExistingApp));
+
service.getData$().subscribe(result => {
expect(result).toEqual([mockParameters, mockCustomer, mockRouteParams, mockFormConfiguration, mockValidateExistingApp]);
});
});
+
it('should retrieve the customer modification form configuration', () => {
spyOn(storageService, 'getParameters').and.returnValue(of(mockParameters));
+
service.getFormConfiguration().subscribe(result => {
expect(result).toEqual(mockFormConfiguration);
});
});
+
it('should request the mocked clients endpoint', () => {
service.getClients$().subscribe(result => {
expect(result).toEqual(mockClients);
});
+
const request = httpTestingController.expectOne('v1/customer-modification/clients.json');
expect(request.request.method.toLowerCase()).toBe('get');
expect(request.request.headers.get('no-error')).toBe('true');
request.flush(mockClients);
});
+
it('should request in-progress customer context using post service method', () => {
const mockClient = mockClients[0];
spyOn(storageService, 'getHttpMethod').and.returnValue('POST');
+
service.getInProgressCustomer$('12345').subscribe(result => {
expect(result).toEqual({ client: mockClient });
});
+
const request = httpTestingController.expectOne('v1/customer-modification/in-progress-context');
expect(request.request.method.toLowerCase()).toBe('post');
expect(request.request.body).toEqual({ customerId: '12345' });
request.flush({ client: mockClient });
});
+
it('should return an empty array when the clients endpoint fails', () => {
service.getClients$().subscribe(result => {
expect(result).toEqual([]);
});
+
const request = httpTestingController.expectOne('v1/customer-modification/clients.json');
request.flush('error', { status: 500, statusText: 'Server Error' });
});
});
+});
diff --git a/src/app/features/customer-modification/services/customer-modification.service.ts b/src/app/features/customer-modification/services/customer-modification.service.ts
new file mode 100644
index 00000000..5690093b
--- /dev/null
+++ b/src/app/features/customer-modification/services/customer-modification.service.ts
@@ -0,0 +1,87 @@
+import { Injectable, inject } from '@angular/core';
+import { HttpClient, HttpHeaders } from '@angular/common/http';
+import { catchError, combineLatest, map, Observable, of } from 'rxjs';
+import { UtilsApi } from '@sanes-hipdig/lf-ng-50084125-front-compones';
+import { StorageService } from '../../../core/services';
+import { HttpHeadersEnum, HttpMethodEnum } from '../../../shared/enums/http-common.enum';
+import {
CustomerModificationClient,
CustomerModificationInProgressResponse,
+} from '../../../shared/models/api/common/customer-modification.model';
+
+/**
CustomerModificationService
*/
+@Injectable({
providedIn: 'root',
+})
+export class CustomerModificationService {
private readonly _http = inject(HttpClient);
private readonly _utilsApiService = inject(UtilsApi);
private readonly _storageService = inject(StorageService);
+
/**
get Data
@returns An observable that emits the combined data from parameters, customer, route parameters, and form configuration.
*/
getData$(): Observable<any> {
return combineLatest([
this._storageService.getParameters(),
this._storageService.getCustomer(),
this._storageService.getRouteParams(),
this.getFormConfiguration(),
this._storageService.getValidateExistingApp(),
]);
}
/**
get Form Configuration
@returns An observable that emits the form configuration for customer modification.
*/
getFormConfiguration(): Observable<any> {
return this._storageService
.getParameters()
.pipe(map(response => response?.mortgagesOriginationCatalogue.parameter.customerModification));
}
+
/**
get Clients
@returns An observable that emits an array of customer modification clients.
*/
getClients$(): Observable<CustomerModificationClient[]> {
const config = {
url: 'v1/customer-modification/clients',
urlMock: 'v1/customer-modification/clients',
mocked: true,
httpMethod: HttpMethodEnum.get,
};
const headers = new HttpHeaders().set(HttpHeadersEnum.noError, 'true');
+
return this._http
.request<CustomerModificationClient[]>(config.httpMethod, this._utilsApiService.getEndPointUrl(config), { headers })
.pipe(catchError(() => of([])));
}
+
/**
Retrieves the in-progress customer modification client data.
@param customerId string applicant id to identify the context
@returns An observable with the full client in progress.
*/
getInProgressCustomer$(customerId: string): Observable<CustomerModificationInProgressResponse> {
const config = {
url: 'v1/customer-modification/in-progress-context',
urlMock: 'v1/customer-modification/in-progress-context',
mocked: true,
httpMethod: HttpMethodEnum.post,
};
+
const body = { customerId };
+
return this._http
.request<CustomerModificationInProgressResponse>(
this._storageService.getHttpMethod(config),
this._utilsApiService.getEndPointUrl(config),
{ body }
)
.pipe(catchError(() => of({ client: null })));
}
+}
diff --git a/src/app/features/distributor/components/purpose/purpose.component.html b/src/app/features/distributor/components/purpose/purpose.component.html
index 6ca48596..dace5463 100644
--- a/src/app/features/distributor/components/purpose/purpose.component.html
+++ b/src/app/features/distributor/components/purpose/purpose.component.html
@@ -42,6 +42,16 @@
</div>
</div>
}
@if (purpose.isCustomerModificationInProgress) {
<div
[id]="'in-progress-purpose-' + purpose.title"
class="purpose-content-top-in-progress-hd"
<div class="purpose-content-top-in-progress-hd-text">
{{ 'DISTRIBUTOR.IN_PROGRESS_HD' | translate }}
</div>
</div>
}
</div>
<div class="purpose-content-description">
{{ purpose.description | translate }}
diff --git a/src/app/features/distributor/distributor.component.ts b/src/app/features/distributor/distributor.component.ts
index f04e4602..23a35c50 100644
--- a/src/app/features/distributor/distributor.component.ts
+++ b/src/app/features/distributor/distributor.component.ts
@@ -148,6 +148,10 @@ export class DistributorComponent implements OnInit, OnDestroy {
this._handleNovationCase();
return;
}
if (this.validatateExistingApplication.isCustomerModification) {
this._handleCustomerModificationCase();
return;
}
this.sections.forEach((section) => {
this.isFirstMatchFound = false;
section.purposes.forEach((purpose: IPurpose) => {
@@ -210,6 +214,22 @@ export class DistributorComponent implements OnInit, OnDestroy {
});
}
/**
Handles the customer modification case by setting the appropriate flags and disabling other purposes.
*/
private _handleCustomerModificationCase(): void {
this.sections.forEach((section) => {
section.purposes.forEach((purpose: IPurpose) => {
if (purpose.path === '/customer-modification') {
purpose.isCustomerModificationInProgress = true;
} else if (!purpose.externalLink?.key) {
purpose.disabled = true;
purpose.icon = 'security-close';
}
});
});
}
+
/**
Set the mortgage information in the purposes
/
diff --git a/src/app/shared/models/api/common/customer-modification-change.model.ts b/src/app/shared/models/api/common/customer-modification-change.model.ts
new file mode 100644
index 00000000..9be02b22
--- /dev/null
+++ b/src/app/shared/models/api/common/customer-modification-change.model.ts
@@ -0,0 +1,16 @@
+/*
Defines the structure of a customer modification change, including the field key, label, old value, and new value.
*/
+export interface CustomerModificationOption {
value: boolean | number | string;
label: string;
+}
+/**
Defines the structure of a customer modification change, including the field key, label, old value, and new value.
*/
+export interface CustomerModificationChange {
fieldKey: string;
label: string;
oldValue: unknown;
newValue: unknown;
+}
diff --git a/src/app/shared/models/api/common/customer-modification.model.ts b/src/app/shared/models/api/common/customer-modification.model.ts
new file mode 100644
index 00000000..4546c989
--- /dev/null
+++ b/src/app/shared/models/api/common/customer-modification.model.ts
@@ -0,0 +1,23 @@
+/**
CustomerModificationClient interface – represents a bank client available for modification.
*/
+export interface CustomerModificationClient {
id: number;
fullName: string;
document: string;
email: string;
phone: string;
accountNumber: string;
accountType: string;
branchOffice: string;
transferLimit: number;
notificationsEnabled: boolean;
preferredContactMethod: string;
+}
+
+/**
Response for in-progress customer modification context.
*/
+export interface CustomerModificationInProgressResponse {
client: CustomerModificationClient | null;
+}
diff --git a/src/app/shared/models/api/common/purpose-group.model.ts b/src/app/shared/models/api/common/purpose-group.model.ts
index f8f00af1..3fe8024a 100644
--- a/src/app/shared/models/api/common/purpose-group.model.ts
+++ b/src/app/shared/models/api/common/purpose-group.model.ts
@@ -138,6 +138,7 @@ export interface IPurpose {
nhb: BreadcrumbLink[];
};
isNovation?: boolean;
isCustomerModificationInProgress?: boolean;
}
/
diff --git a/src/app/shared/models/api/parameters/customer-modification-parameters.model.ts b/src/app/shared/models/api/parameters/customer-modification-parameters.model.ts
new file mode 100644
index 00000000..43f137c2
--- /dev/null
+++ b/src/app/shared/models/api/parameters/customer-modification-parameters.model.ts
@@ -0,0 +1,14 @@
+/

ParametersCustomerModification interface
*/
+export interface ParametersCustomerModification {
form: CustomerModificationForm;
+}
+
+/**
CustomerModificationForm
*/
+export interface CustomerModificationForm {
fields: any[];
optionsData: any;
+}
diff --git a/src/app/shared/models/api/parameters/parameters-response.model.ts b/src/app/shared/models/api/parameters/parameters-response.model.ts
index a98260d1..f77b00c5 100644
--- a/src/app/shared/models/api/parameters/parameters-response.model.ts
+++ b/src/app/shared/models/api/parameters/parameters-response.model.ts
@@ -1,6 +1,7 @@
import { CallMeParameters } from '../../call-me/call-me.model';
import { Distributor } from '../common/purpose-group.model';
import { ParametersAttracting } from './attracting-parameters.model';
+import { ParametersCustomerModification } from './customer-modification-parameters.model';
import { ParametersNovation } from './novation-parameters.model';
import { ParametersOtherObjetive } from './other-objetive-parameters.model';
@@ -189,6 +190,7 @@ export interface Parameter {
attracting: ParametersAttracting;
otherObjetive?: ParametersOtherObjetive;
novation?: ParametersNovation;

customerModification?: ParametersCustomerModification;
opinator: any;
globals: ParametersGlobals;
appraisal: ParametersAppraisal;
diff --git a/src/app/shared/models/api/validate-existing-aplication/validate-existing-application.model.ts b/src/app/shared/models/api/validate-existing-aplication/validate-existing-application.model.ts
index baab5df6..d0cb2972 100644
--- a/src/app/shared/models/api/validate-existing-aplication/validate-existing-application.model.ts
+++ b/src/app/shared/models/api/validate-existing-aplication/validate-existing-application.model.ts
@@ -12,6 +12,7 @@ export interface ValidatateExistingApplication {
purpose?: string | null;
fluxTypeLast?: string;
isNovation?: boolean;
isCustomerModification?: boolean;
}
/**
*
diff --git a/src/assets/i18n/ca.json b/src/assets/i18n/ca.json
index ae643c1a..42df33fc 100644
--- a/src/assets/i18n/ca.json
+++ b/src/assets/i18n/ca.json
@@ -1042,6 +1042,10 @@
"TITLE": "Modificar préstec hipotecari",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modificar client bancari",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificat de deute zero i cancel·lació registral",
"SUMMARY": ""
@@ -3372,6 +3376,69 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "No hi ha clients disponibles per modificar",
"NO_CLIENTS_DESC": "Ara mateix no hi ha clients bancaris disponibles per a aquesta operació.",
"NO_CLIENTS_DESC": "Ara mateix no hi ha clients bancaris disponibles per a aquesta operaci\u00f3.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Seleccionar client",
"LABEL_2": "Modificar dades",
"LABEL_3": "Resum"
},
"LABEL": "Modificar client",
"SELECT_CLIENT": {
"TITLE": "Selecciona el client bancari",
"DESCRIPTION": "Tria el client que vols modificar"
},
"MODIFY_DATA": {
"TITLE": "Modifica les dades del client",
"DESCRIPTION": "Actualitza els camps necessaris i continua"
},
"FULL_NAME": { "LABEL": "Nom complet" },
"EMAIL": { "LABEL": "Correu electrònic" },
"PHONE": { "LABEL": "Telèfon" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Tipus de compte" },
"BRANCH_OFFICE": { "LABEL": "Oficina" },
"TRANSFER_LIMIT": { "LABEL": "Límit de transferència" },
"NOTIFICATIONS": { "LABEL": "Notificacions", "TITLE": "Notificacions" },
"PREFERRED_CONTACT": { "LABEL": "Canal de contacte preferit" }
}
},
"SUMMARY": {
"TITLE": "Resum de canvis",
"DISCLAIMER": "Revisa els canvis abans de confirmar la modificació",
"NO_CHANGES": "No s'han detectat canvis"
},
"MODAL": {
"TITLE": "Canvis desats",
"TEXT": "La modificació del client s'ha fet correctament",
"ACCEPT": "Acceptar"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Compte nòmina",
"SAVINGS": "Compte d'estalvi",
"BUSINESS": "Compte d'empresa",
"PREMIUM": "Compte Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "Correu electrònic",
"PHONE": "Telèfon",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Aquest camp no pot contenir números",
"EMAIL_FORMAT": "Introdueix un correu electrònic vàlid",
"ONLY_NUMBERS": "Aquest camp només permet números",
"MAX_NINE_DIGITS": "Aquest camp admet un màxim de 9 dígits",
"IBAN_FORMAT": "Introdueix un IBAN vàlid",
"TRANSFER_LIMIT_RANGE": "El límit ha d'estar entre 0 i 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "La meva vivenda - Simuladors - Contractació hipoteca",
"ATTRACTING_TITLE": "Millora hipoteca",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simular la teva hipoteca",
"NOVATION_TITLE": "Modificar préstec hipotecari",
"CUSTOMER_MODIFICATION_TITLE": "Modificar client bancari",
"LOAN": "Préstec",
"DETAIL": "Detall",
"MY_HOME": "La meva vivenda"
diff --git a/src/assets/i18n/en.json b/src/assets/i18n/en.json
index 7429d9e4..d974107c 100644
--- a/src/assets/i18n/en.json
+++ b/src/assets/i18n/en.json
@@ -1042,6 +1042,10 @@
"TITLE": "Modify mortgage loan",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modify bank customer",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificate of zero debt and cancellation of registration",
"SUMMARY": ""
@@ -3372,6 +3376,87 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "No clients available to modify",
"NO_CLIENTS_DESC": "There are no bank customers available for this operation right now.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Select customer",
"LABEL_2": "Modify data",
"LABEL_3": "Summary"
},
"LABEL": "Modify customer",
"SELECT_CLIENT": {
"TITLE": "Select bank customer",
"DESCRIPTION": "Choose the customer you want to modify"
},
"MODIFY_DATA": {
"TITLE": "Modify customer data",
"DESCRIPTION": "Update the required fields and continue"
},
"FULL_NAME": {
"LABEL": "Full name"
},
"EMAIL": {
"LABEL": "Email"
},
"PHONE": {
"LABEL": "Phone"
},
"ACCOUNT_NUMBER": {
"LABEL": "IBAN"
},
"ACCOUNT_TYPE": {
"LABEL": "Account type"
},
"BRANCH_OFFICE": {
"LABEL": "Branch office"
},
"TRANSFER_LIMIT": {
"LABEL": "Transfer limit"
},
"NOTIFICATIONS": {
"LABEL": "Notifications",
"TITLE": "Notifications"
},
"PREFERRED_CONTACT": {
"LABEL": "Preferred contact channel"
}
}
},
"SUMMARY": {
"TITLE": "Summary of changes",
"DISCLAIMER": "Review the changes before confirming the modification",
"NO_CHANGES": "No changes were detected"
},
"MODAL": {
"TITLE": "Changes saved",
"TEXT": "Customer modification was completed successfully",
"ACCEPT": "Accept"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Payroll account",
"SAVINGS": "Savings account",
"BUSINESS": "Business account",
"PREMIUM": "Premium account"
},
"PREFERRED_CONTACT": {
"EMAIL": "Email",
"PHONE": "Phone",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "This field cannot contain numbers",
"EMAIL_FORMAT": "Enter a valid email address",
"ONLY_NUMBERS": "This field only allows numbers",
"MAX_NINE_DIGITS": "This field allows a maximum of 9 digits",
"IBAN_FORMAT": "Enter a valid IBAN",
"TRANSFER_LIMIT_RANGE": "Limit must be between 0 and 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "My home - Simulators - Mortgage application",
"ATTRACTING_TITLE": "Improve mortgage",
@@ -3379,6 +3464,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simulate your mortgage",
"NOVATION_TITLE": "Modify mortgage loan",
"CUSTOMER_MODIFICATION_TITLE": "Modify bank customer",
"LOAN": "Loan",
"DETAIL": "Detail",
"MY_HOME": "My home"
diff --git a/src/assets/i18n/es.json b/src/assets/i18n/es.json
index d38d659c..1cd15aea 100644
--- a/src/assets/i18n/es.json
+++ b/src/assets/i18n/es.json
@@ -1042,6 +1042,10 @@
"TITLE": "Modificar préstamo hipotecario",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modificar cliente bancario",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificado de deuda cero y cancelación registral",
"SUMMARY": ""
@@ -3372,6 +3376,87 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "No hay clientes disponibles para modificar",
"NO_CLIENTS_DESC": "Ahora mismo no hay clientes bancarios disponibles para esta operaci\u00f3n.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Seleccionar cliente",
"LABEL_2": "Modificar datos",
"LABEL_3": "Resumen"
},
"LABEL": "Modificar cliente",
"SELECT_CLIENT": {
"TITLE": "Selecciona el cliente bancario",
"DESCRIPTION": "Elige el cliente que quieres modificar"
},
"MODIFY_DATA": {
"TITLE": "Modifica los datos del cliente",
"DESCRIPTION": "Actualiza los campos necesarios y continúa"
},
"FULL_NAME": {
"LABEL": "Nombre completo"
},
"EMAIL": {
"LABEL": "Correo electrónico"
},
"PHONE": {
"LABEL": "Teléfono"
},
"ACCOUNT_NUMBER": {
"LABEL": "IBAN"
},
"ACCOUNT_TYPE": {
"LABEL": "Tipo de cuenta"
},
"BRANCH_OFFICE": {
"LABEL": "Oficina"
},
"TRANSFER_LIMIT": {
"LABEL": "Límite de transferencia"
},
"NOTIFICATIONS": {
"LABEL": "Notificaciones"
},
"PREFERRED_CONTACT": {
"LABEL": "Método de contacto preferido"
}
}
},
"SUMMARY": {
"TITLE": "Resumen de datos modificados",
"DISCLAIMER": "Esta información es una previsualización de los datos modificados.",
"TITLE_COMPARATOR": "Comparador de datos modificados",
"NO_CHANGES": "No se han detectado cambios"
},
"MODAL": {
"TITLE": "Cambios guardados",
"TEXT": "La modificación del cliente se ha realizado correctamente",
"ACCEPT": "Aceptar"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Cuenta Nómina",
"SAVINGS": "Cuenta Ahorro",
"BUSINESS": "Cuenta Empresa",
"PREMIUM": "Cuenta Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "Correo electrónico",
"PHONE": "Teléfono",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Este campo no puede contener números",
"EMAIL_FORMAT": "Introduce un correo electrónico válido",
"ONLY_NUMBERS": "Este campo solo permite números",
"MAX_NINE_DIGITS": "Este campo admite un máximo de 9 dígitos",
"IBAN_FORMAT": "Introduce un IBAN válido",
"TRANSFER_LIMIT_RANGE": "El límite debe estar entre 0 y 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "Mi vivienda - Simuladores - Contratación hipoteca",
"ATTRACTING_TITLE": "Mejora hipoteca",
@@ -3379,6 +3464,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simular tu hipoteca",
"NOVATION_TITLE": "Modificar préstamo hipotecario",
"CUSTOMER_MODIFICATION_TITLE": "Modificar cliente bancario",
"LOAN": "Préstamo",
"DETAIL": "Detalle",
"MY_HOME": "Mi vivienda"
diff --git a/src/assets/i18n/eu.json b/src/assets/i18n/eu.json
index 944d6d6f..c030bee8 100644
--- a/src/assets/i18n/eu.json
+++ b/src/assets/i18n/eu.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Une honetan ez dago eragiketa honetarako bankuko bezerorik erabilgarri.",
"STEP_OF": " urratsa"
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Hipoteka mailegua aldatu",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Bankuko bezeroa aldatu",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Zero zor ziurtagiria eta matrikula baliogabetzea",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "Ez dago aldatzeko bezero erabilgarririk",
"NO_CLIENTS_DESC": "Une honetan ez dago eragiketa honetarako bankuko bezerorik erabilgarri.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Bezeroa hautatu",
"LABEL_2": "Datuak aldatu",
"LABEL_3": "Laburpena"
},
"LABEL": "Bezeroa aldatu",
"SELECT_CLIENT": {
"TITLE": "Hautatu bankuko bezeroa",
"DESCRIPTION": "Aukeratu aldatu nahi duzun bezeroa"
},
"MODIFY_DATA": {
"TITLE": "Bezeroaren datuak aldatu",
"DESCRIPTION": "Eguneratu beharrezko eremuak eta jarraitu"
},
"FULL_NAME": { "LABEL": "Izen-abizenak" },
"EMAIL": { "LABEL": "Helbide elektronikoa" },
"PHONE": { "LABEL": "Telefonoa" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Kontu mota" },
"BRANCH_OFFICE": { "LABEL": "Bulegoa" },
"TRANSFER_LIMIT": { "LABEL": "Transferentzia muga" },
"NOTIFICATIONS": { "LABEL": "Jakinarazpenak", "TITLE": "Jakinarazpenak" },
"PREFERRED_CONTACT": { "LABEL": "Hobetsitako harreman-kanala" }
}
},
"SUMMARY": {
"TITLE": "Aldaketen laburpena",
"DISCLAIMER": "Berrikusi aldaketak baieztatu aurretik",
"NO_CHANGES": "Ez da aldaketarik hauteman"
},
"MODAL": {
"TITLE": "Aldaketak gordeta",
"TEXT": "Bezeroaren aldaketa ondo burutu da",
"ACCEPT": "Onartu"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Nomina kontua",
"SAVINGS": "Aurrezki kontua",
"BUSINESS": "Enpresa kontua",
"PREMIUM": "Premium kontua"
},
"PREFERRED_CONTACT": {
"EMAIL": "Helbide elektronikoa",
"PHONE": "Telefonoa",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Eremu honek ezin du zenbakirik izan",
"EMAIL_FORMAT": "Sartu baliozko helbide elektroniko bat",
"ONLY_NUMBERS": "Eremu honek zenbakiak bakarrik onartzen ditu",
"MAX_NINE_DIGITS": "Eremu honek gehienez 9 digitu onartzen ditu",
"IBAN_FORMAT": "Sartu baliozko IBAN bat",
"TRANSFER_LIMIT_RANGE": "Mugak 0 eta 3000 artean egon behar du"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "Nire etxebizitza - Simulagailuak - Hipoteka kontratazioa",
"ATTRACTING_TITLE": "Hobetu hipoteka",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simulatu zure hipoteka",
"NOVATION_TITLE": "Hipoteka mailegu aldatu",
"CUSTOMER_MODIFICATION_TITLE": "Bankuko bezeroa aldatu",
"LOAN": "Mailegu",
"DETAIL": "Xehetasuna",
"MY_HOME": "Nire etxebizitza"
diff --git a/src/assets/i18n/fr.json b/src/assets/i18n/fr.json
index 8beb5537..f2873b99 100644
--- a/src/assets/i18n/fr.json
+++ b/src/assets/i18n/fr.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Aucun client bancaire n'est disponible pour cette op\u00e9ration pour le moment.",
"STEP_OF": "Étape "
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Modifier un prêt hypothécaire",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modifier le client bancaire",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificat de dette zéro et annulation de l'enregistrement",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "Aucun client disponible à modifier",
"NO_CLIENTS_DESC": "Aucun client bancaire n'est disponible pour cette opération pour le moment.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Sélectionner un client",
"LABEL_2": "Modifier les données",
"LABEL_3": "Résumé"
},
"LABEL": "Modifier le client",
"SELECT_CLIENT": {
"TITLE": "Sélectionnez le client bancaire",
"DESCRIPTION": "Choisissez le client à modifier"
},
"MODIFY_DATA": {
"TITLE": "Modifier les données du client",
"DESCRIPTION": "Mettez à jour les champs requis puis continuez"
},
"FULL_NAME": { "LABEL": "Nom complet" },
"EMAIL": { "LABEL": "E-mail" },
"PHONE": { "LABEL": "Téléphone" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Type de compte" },
"BRANCH_OFFICE": { "LABEL": "Agence" },
"TRANSFER_LIMIT": { "LABEL": "Limite de virement" },
"NOTIFICATIONS": { "LABEL": "Notifications", "TITLE": "Notifications" },
"PREFERRED_CONTACT": { "LABEL": "Canal de contact préféré" }
}
},
"SUMMARY": {
"TITLE": "Résumé des changements",
"DISCLAIMER": "Vérifiez les changements avant de confirmer",
"NO_CHANGES": "Aucun changement détecté"
},
"MODAL": {
"TITLE": "Changements enregistrés",
"TEXT": "La modification du client a été effectuée avec succès",
"ACCEPT": "Accepter"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Compte salaire",
"SAVINGS": "Compte épargne",
"BUSINESS": "Compte entreprise",
"PREMIUM": "Compte premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "E-mail",
"PHONE": "Téléphone",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Ce champ ne peut pas contenir de chiffres",
"EMAIL_FORMAT": "Saisissez une adresse e-mail valide",
"ONLY_NUMBERS": "Ce champ n'accepte que des chiffres",
"MAX_NINE_DIGITS": "Ce champ accepte un maximum de 9 chiffres",
"IBAN_FORMAT": "Saisissez un IBAN valide",
"TRANSFER_LIMIT_RANGE": "La limite doit être comprise entre 0 et 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "Mon logement - Simulateurs - Demande de prêt hypothécaire",
"ATTRACTING_TITLE": "Améliorer l'hypothèque",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simuler votre prêt hypothécaire",
"NOVATION_TITLE": "Modifier prêt hypothécaire",
"CUSTOMER_MODIFICATION_TITLE": "Modifier le client bancaire",
"LOAN": "Prêt",
"DETAIL": "Détail",
"MY_HOME": "Mon logement"
diff --git a/src/assets/i18n/gl.json b/src/assets/i18n/gl.json
index c055fff8..3a20e012 100644
--- a/src/assets/i18n/gl.json
+++ b/src/assets/i18n/gl.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Nestes momentos non hai clientes bancarios dispo\u00f1ibles para esta operaci\u00f3n.",
"STEP_OF": "Paso "
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Modificar préstamo hipotecario",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modificar cliente bancario",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificado de débeda cero e cancelación de rexistro",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "Non hai clientes dispoñibles para modificar",
"NO_CLIENTS_DESC": "Nestes momentos non hai clientes bancarios dispoñibles para esta operación.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Seleccionar cliente",
"LABEL_2": "Modificar datos",
"LABEL_3": "Resumo"
},
"LABEL": "Modificar cliente",
"SELECT_CLIENT": {
"TITLE": "Selecciona o cliente bancario",
"DESCRIPTION": "Escolle o cliente que queres modificar"
},
"MODIFY_DATA": {
"TITLE": "Modifica os datos do cliente",
"DESCRIPTION": "Actualiza os campos necesarios e continúa"
},
"FULL_NAME": { "LABEL": "Nome completo" },
"EMAIL": { "LABEL": "Correo electrónico" },
"PHONE": { "LABEL": "Teléfono" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Tipo de conta" },
"BRANCH_OFFICE": { "LABEL": "Oficina" },
"TRANSFER_LIMIT": { "LABEL": "Límite de transferencia" },
"NOTIFICATIONS": { "LABEL": "Notificacións", "TITLE": "Notificacións" },
"PREFERRED_CONTACT": { "LABEL": "Canle de contacto preferida" }
}
},
"SUMMARY": {
"TITLE": "Resumo de cambios",
"DISCLAIMER": "Revisa os cambios antes de confirmar a modificación",
"NO_CHANGES": "Non se detectaron cambios"
},
"MODAL": {
"TITLE": "Cambios gardados",
"TEXT": "A modificación do cliente realizouse correctamente",
"ACCEPT": "Aceptar"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Conta Nómina",
"SAVINGS": "Conta Aforro",
"BUSINESS": "Conta Empresa",
"PREMIUM": "Conta Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "Correo electrónico",
"PHONE": "Teléfono",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Este campo non pode conter números",
"EMAIL_FORMAT": "Introduce un correo electrónico válido",
"ONLY_NUMBERS": "Este campo só permite números",
"MAX_NINE_DIGITS": "Este campo admite un máximo de 9 díxitos",
"IBAN_FORMAT": "Introduce un IBAN válido",
"TRANSFER_LIMIT_RANGE": "O límite debe estar entre 0 e 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "A miña vivenda - Simuladores - Contratación hipoteca",
"ATTRACTING_TITLE": "Mellorar hipoteca",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simular a túa hipoteca",
"NOVATION_TITLE": "Modificar préstamo hipotecario",
"CUSTOMER_MODIFICATION_TITLE": "Modificar cliente bancario",
"LOAN": "Préstamo",
"DETAIL": "Detalle",
"MY_HOME": "A miña vivenda"
diff --git a/src/assets/i18n/it.json b/src/assets/i18n/it.json
index ef5bb333..61f80a60 100644
--- a/src/assets/i18n/it.json
+++ b/src/assets/i18n/it.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Al momento non ci sono clienti bancari disponibili per questa operazione.",
"STEP_OF": "Fase "
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Modificare il mutuo ipotecario",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modificare cliente bancario",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificato di debito zero e cancellazione della registrazione",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "Nessun cliente disponibile da modificare",
"NO_CLIENTS_DESC": "Al momento non ci sono clienti bancari disponibili per questa operazione.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Seleziona cliente",
"LABEL_2": "Modifica dati",
"LABEL_3": "Riepilogo"
},
"LABEL": "Modifica cliente",
"SELECT_CLIENT": {
"TITLE": "Seleziona il cliente bancario",
"DESCRIPTION": "Scegli il cliente che vuoi modificare"
},
"MODIFY_DATA": {
"TITLE": "Modifica i dati del cliente",
"DESCRIPTION": "Aggiorna i campi necessari e continua"
},
"FULL_NAME": { "LABEL": "Nome completo" },
"EMAIL": { "LABEL": "Email" },
"PHONE": { "LABEL": "Telefono" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Tipo di conto" },
"BRANCH_OFFICE": { "LABEL": "Filiale" },
"TRANSFER_LIMIT": { "LABEL": "Limite di trasferimento" },
"NOTIFICATIONS": { "LABEL": "Notifiche", "TITLE": "Notifiche" },
"PREFERRED_CONTACT": { "LABEL": "Canale di contatto preferito" }
}
},
"SUMMARY": {
"TITLE": "Riepilogo modifiche",
"DISCLAIMER": "Controlla le modifiche prima di confermare",
"NO_CHANGES": "Nessuna modifica rilevata"
},
"MODAL": {
"TITLE": "Modifiche salvate",
"TEXT": "La modifica del cliente è stata completata correttamente",
"ACCEPT": "Accetta"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Conto stipendio",
"SAVINGS": "Conto risparmio",
"BUSINESS": "Conto aziendale",
"PREMIUM": "Conto Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "Email",
"PHONE": "Telefono",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Questo campo non può contenere numeri",
"EMAIL_FORMAT": "Inserisci un indirizzo email valido",
"ONLY_NUMBERS": "Questo campo consente solo numeri",
"MAX_NINE_DIGITS": "Questo campo consente un massimo di 9 cifre",
"IBAN_FORMAT": "Inserisci un IBAN valido",
"TRANSFER_LIMIT_RANGE": "Il limite deve essere compreso tra 0 e 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "La mia casa - Simulatori - Contratto mutuo",
"ATTRACTING_TITLE": "Migliora mutuo",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simula il tuo mutuo",
"NOVATION_TITLE": "Modificare mutuo ipotecario",
"CUSTOMER_MODIFICATION_TITLE": "Modificare cliente bancario",
"LOAN": "Prestito",
"DETAIL": "Dettaglio",
"MY_HOME": "La mia casa"
diff --git a/src/assets/i18n/pb.json b/src/assets/i18n/pb.json
index d498ca8e..ddcd6364 100644
--- a/src/assets/i18n/pb.json
+++ b/src/assets/i18n/pb.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Agora nao ha clientes bancarios disponiveis para esta operacao.",
"STEP_OF": "Paso "
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Modificar préstamo hipotecario",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modificar cliente bancario",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certificado de deuda cero y cancelación registral",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "No hay clientes disponibles para modificar",
"NO_CLIENTS_DESC": "Agora nao ha clientes bancarios disponiveis para esta operacao.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Seleccionar cliente",
"LABEL_2": "Modificar datos",
"LABEL_3": "Resumen"
},
"LABEL": "Modificar cliente",
"SELECT_CLIENT": {
"TITLE": "Selecciona el cliente bancario",
"DESCRIPTION": "Elige el cliente que quieres modificar"
},
"MODIFY_DATA": {
"TITLE": "Modifica los datos del cliente",
"DESCRIPTION": "Actualiza los campos necesarios y continúa"
},
"FULL_NAME": { "LABEL": "Nombre completo" },
"EMAIL": { "LABEL": "Correo electrónico" },
"PHONE": { "LABEL": "Teléfono" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Tipo de cuenta" },
"BRANCH_OFFICE": { "LABEL": "Oficina" },
"TRANSFER_LIMIT": { "LABEL": "Límite de transferencia" },
"NOTIFICATIONS": { "LABEL": "Notificaciones", "TITLE": "Notificaciones" },
"PREFERRED_CONTACT": { "LABEL": "Método de contacto preferido" }
}
},
"SUMMARY": {
"TITLE": "Resumen de cambios",
"DISCLAIMER": "Revisa los cambios antes de confirmar la modificación",
"NO_CHANGES": "No se han detectado cambios"
},
"MODAL": {
"TITLE": "Cambios guardados",
"TEXT": "La modificación del cliente se ha realizado correctamente",
"ACCEPT": "Aceptar"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Cuenta Nómina",
"SAVINGS": "Cuenta Ahorro",
"BUSINESS": "Cuenta Empresa",
"PREMIUM": "Cuenta Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "Correo electrónico",
"PHONE": "Teléfono",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Este campo no puede contener números",
"EMAIL_FORMAT": "Introduce un correo electrónico válido",
"ONLY_NUMBERS": "Este campo solo permite números",
"MAX_NINE_DIGITS": "Este campo admite un máximo de 9 dígitos",
"IBAN_FORMAT": "Introduce un IBAN válido",
"TRANSFER_LIMIT_RANGE": "El límite debe estar entre 0 y 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "Mi vivienda - Simuladores - Contratación hipoteca",
"ATTRACTING_TITLE": "Mejora hipoteca",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simular tu hipoteca",
"NOVATION_TITLE": "Modificar préstamo hipotecario",
"CUSTOMER_MODIFICATION_TITLE": "Modificar cliente bancario",
"LOAN": "Préstamo",
"DETAIL": "Detalle",
"MY_HOME": "Mi vivienda"
diff --git a/src/assets/i18n/pl.json b/src/assets/i18n/pl.json
index 2b746e69..705ff247 100644
--- a/src/assets/i18n/pl.json
+++ b/src/assets/i18n/pl.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Obecnie brak klient\u00f3w bankowych dost\u0119pnych dla tej operacji.",
"STEP_OF": "Krok "
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Modyfikacja kredytu hipotecznego",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modyfikacja klienta bankowego",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certyfikat zerowego zadłużenia i wykreślenie rejestracji",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "Brak klientów dostępnych do modyfikacji",
"NO_CLIENTS_DESC": "Obecnie brak klientów bankowych dostępnych dla tej operacji.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Wybierz klienta",
"LABEL_2": "Zmień dane",
"LABEL_3": "Podsumowanie"
},
"LABEL": "Zmodyfikuj klienta",
"SELECT_CLIENT": {
"TITLE": "Wybierz klienta bankowego",
"DESCRIPTION": "Wybierz klienta, którego chcesz zmodyfikować"
},
"MODIFY_DATA": {
"TITLE": "Zmień dane klienta",
"DESCRIPTION": "Zaktualizuj wymagane pola i kontynuuj"
},
"FULL_NAME": { "LABEL": "Imię i nazwisko" },
"EMAIL": { "LABEL": "E-mail" },
"PHONE": { "LABEL": "Telefon" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Typ konta" },
"BRANCH_OFFICE": { "LABEL": "Oddział" },
"TRANSFER_LIMIT": { "LABEL": "Limit przelewu" },
"NOTIFICATIONS": { "LABEL": "Powiadomienia", "TITLE": "Powiadomienia" },
"PREFERRED_CONTACT": { "LABEL": "Preferowany kanał kontaktu" }
}
},
"SUMMARY": {
"TITLE": "Podsumowanie zmian",
"DISCLAIMER": "Sprawdź zmiany przed potwierdzeniem",
"NO_CHANGES": "Nie wykryto zmian"
},
"MODAL": {
"TITLE": "Zmiany zapisane",
"TEXT": "Modyfikacja klienta została wykonana poprawnie",
"ACCEPT": "Akceptuj"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Konto wynagrodzenia",
"SAVINGS": "Konto oszczędnościowe",
"BUSINESS": "Konto firmowe",
"PREMIUM": "Konto Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "E-mail",
"PHONE": "Telefon",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "To pole nie może zawierać cyfr",
"EMAIL_FORMAT": "Wprowadź poprawny adres e-mail",
"ONLY_NUMBERS": "To pole dopuszcza tylko cyfry",
"MAX_NINE_DIGITS": "To pole dopuszcza maksymalnie 9 cyfr",
"IBAN_FORMAT": "Wprowadź poprawny IBAN",
"TRANSFER_LIMIT_RANGE": "Limit musi mieścić się w zakresie od 0 do 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "Mój dom - Symulatory - Zawarcie hipoteki",
"ATTRACTING_TITLE": "Popraw hipotekę",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Symuluj swoją hipotekę",
"NOVATION_TITLE": "Zmodyfikuj kredyt hipoteczny",
"CUSTOMER_MODIFICATION_TITLE": "Modyfikacja klienta bankowego",
"LOAN": "Pożyczka",
"DETAIL": "Szczegół",
"MY_HOME": "Mój dom"
diff --git a/src/assets/i18n/pt.json b/src/assets/i18n/pt.json
index 0e7bf384..72ccd599 100644
--- a/src/assets/i18n/pt.json
+++ b/src/assets/i18n/pt.json
@@ -5,6 +5,7 @@
}
},
"STEPPER": {
"NO_CLIENTS_DESC": "Neste momento nao ha clientes bancarios disponiveis para esta operacao.",
"STEP_OF": "Passo "
},
"STEPS": {
@@ -1042,6 +1043,10 @@
"TITLE": "Modificar empréstimo hipotecário",
"SUMMARY": ""
},
"CUSTOMER_MODIFICATION": {
"TITLE": "Modificar cliente bancário",
"SUMMARY": ""
},
"CANCELLATION_OF_REGISTRATION": {
"TITLE": "Certidão de dívida zero e cancelamento do registo",
"SUMMARY": ""
@@ -3372,6 +3377,68 @@
}
}
},
"CUSTOMER_MODIFICATION": {
"NO_CLIENTS": "Não existem clientes disponíveis para modificar",
"NO_CLIENTS_DESC": "Neste momento nao ha clientes bancarios disponiveis para esta operacao.",
"FORM": {
"FIELDS": {
"STEPS_LABELS": {
"LABEL_1": "Selecionar cliente",
"LABEL_2": "Modificar dados",
"LABEL_3": "Resumo"
},
"LABEL": "Modificar cliente",
"SELECT_CLIENT": {
"TITLE": "Selecione o cliente bancário",
"DESCRIPTION": "Escolha o cliente que pretende modificar"
},
"MODIFY_DATA": {
"TITLE": "Modifique os dados do cliente",
"DESCRIPTION": "Atualize os campos necessários e continue"
},
"FULL_NAME": { "LABEL": "Nome completo" },
"EMAIL": { "LABEL": "E-mail" },
"PHONE": { "LABEL": "Telefone" },
"ACCOUNT_NUMBER": { "LABEL": "IBAN" },
"ACCOUNT_TYPE": { "LABEL": "Tipo de conta" },
"BRANCH_OFFICE": { "LABEL": "Balcão" },
"TRANSFER_LIMIT": { "LABEL": "Limite de transferência" },
"NOTIFICATIONS": { "LABEL": "Notificações", "TITLE": "Notificações" },
"PREFERRED_CONTACT": { "LABEL": "Canal de contacto preferido" }
}
},
"SUMMARY": {
"TITLE": "Resumo de alterações",
"DISCLAIMER": "Reveja as alterações antes de confirmar",
"NO_CHANGES": "Não foram detetadas alterações"
},
"MODAL": {
"TITLE": "Alterações guardadas",
"TEXT": "A modificação do cliente foi realizada com sucesso",
"ACCEPT": "Aceitar"
},
"OPTIONS": {
"ACCOUNT_TYPE": {
"PAYROLL": "Conta Salário",
"SAVINGS": "Conta Poupança",
"BUSINESS": "Conta Empresarial",
"PREMIUM": "Conta Premium"
},
"PREFERRED_CONTACT": {
"EMAIL": "E-mail",
"PHONE": "Telefone",
"SMS": "SMS"
}
},
"VALIDATORS": {
"NO_NUMBERS": "Este campo não pode conter números",
"EMAIL_FORMAT": "Introduza um e-mail válido",
"ONLY_NUMBERS": "Este campo apenas permite números",
"MAX_NINE_DIGITS": "Este campo admite no máximo 9 dígitos",
"IBAN_FORMAT": "Introduza um IBAN válido",
"TRANSFER_LIMIT_RANGE": "O limite deve estar entre 0 e 3000"
}
},
"BREADCRUMB": {
"DISTRIBUTOR_TITLE": "Minha casa - Simuladores - Contratação de hipoteca",
"ATTRACTING_TITLE": "Melhorar hipoteca",
@@ -3379,6 +3446,7 @@
"DASHBOARD_TITLE": "Home planner",
"SIMULATION_TITLE": "Simular sua hipoteca",
"NOVATION_TITLE": "Modificar empréstimo hipotecário",
"CUSTOMER_MODIFICATION_TITLE": "Modificar cliente bancário",
"LOAN": "Empréstimo",
"DETAIL": "Detalhe",
"MY_HOME": "Minha casa"
diff --git a/src/mocks/customer-modification-clients.mock.ts b/src/mocks/customer-modification-clients.mock.ts
new file mode 100644
index 00000000..f6907b9d
--- /dev/null
+++ b/src/mocks/customer-modification-clients.mock.ts
@@ -0,0 +1,43 @@
+import { CustomerModificationClient } from 'src/app/shared/models/api/common/customer-modification.model';
+
+export const CUSTOMER_MODIFICATION_CLIENTS_MOCK: CustomerModificationClient[] = [
{
id: 1,
fullName: 'Jesús Félix',
document: '12345678A',
email: 'jesus@test.com',
phone: '600123123',
accountNumber: 'ES6621000418401234567891',
accountType: 'Cuenta Nómina',
branchOffice: 'Madrid Centro',
transferLimit: 3000,
notificationsEnabled: true,
preferredContactMethod: 'EMAIL',
},
{
id: 2,
fullName: 'María García López',
document: '87654321B',
email: 'maria.garcia@test.com',
phone: '611234567',
accountNumber: 'ES7620770024003102575766',
accountType: 'Cuenta Ahorro',
branchOffice: 'Barcelona Norte',
transferLimit: 1500,
notificationsEnabled: false,
preferredContactMethod: 'PHONE',
},
{
id: 3,
fullName: 'Carlos Ruiz Martínez',
document: '11223344C',
email: 'carlos.ruiz@test.com',
phone: '622345678',
accountNumber: 'ES9121000418450200051332',
accountType: 'Cuenta Empresa',
branchOffice: 'Sevilla Este',
transferLimit: 2000,
notificationsEnabled: true,
preferredContactMethod: 'SMS',
},
+];
diff --git a/src/mocks/index.ts b/src/mocks/index.ts
index 228dfde6..5fc3a9ea 100644
--- a/src/mocks/index.ts
+++ b/src/mocks/index.ts
@@ -1,4 +1,5 @@
export * from './calculate-budget-body.mock';
+export * from './customer-modification-clients.mock';
export * from './calculate-simulation-body.mock';
export * from './customer-response.mock';
export * from './mortgage-information-response.mock';
