# README-BANK-JAVA-ACTUALIZADO

---

## Resumen del proyecto

En este proyecto realizamos la creacion de una app bancaria en la cual se implememta el uso de thunder client, springboot, swagger ui y claramente el lenguaje de programacion java
entre las funciones de esta app están la creación de cuentas de ahorro, cuentas corrientes, creacion de usuarios, busqueda de clientes por id, listar todos los clientes, listar las cuentas de un cliente ya sea de ahorro o corriente, realizar un deposito, realizar un retiro, trasnferencia de dinero, historial de transacciones de una cuenta y la aplicacion de sus respectivos intereses de uso, a continuacion les mostrare paso a paso de como se realiza cada uno de estos procesos incluyendo los requisitos para el funcionamiento de la app, su ejecucion, endpoints etc.

---

## Requisitos

* Java 11+ (compatible con Spring Boot usado en el proyecto)
* Maven (o usar el wrapper `mvnw` incluido)
* IDE (IntelliJ, VSCode) con Thunder Client (extensión VSCode) si se quiere probar localmente

---

## Cómo ejecutar la aplicación

1. Descomprime el proyecto y abre la raíz del proyecto.
2. Construir y ejecutar con Maven:

```bash
mvn spring-boot:run
```

Por defecto la aplicación arranca en `http://localhost:8080`.

---


### Modelos principales 

**Customer**

```json
{
  "id": "string",
  "name": "string",
  "email": "string"
}
```

**Account (común)**

```json
{
  "id": "string",
  "owner": { /* Customer */ },
  "balance": { "amount": 0.0, "currency": "USD" },
  "transactions": [ /* Transaction[] */ ]
}
```

**SavingsAccount** extiende Account y tiene `interestRate` (double).

**CheckingAccount** extiende Account y tiene `overdraftLimit` (double).

**Transaction**

```json
{
  "type": "DEP|WDR|TRF|...",
  "amount": { "amount": 100.0, "currency": "USD" },
  "accountId": "string",
  "timestamp": "2025-10-30T..."
}
```

---

### 1. Agregar un cliente

* **URL:** http://localhost:8080/api/bank/api/bank
* **Body (JSON):** `Customer` (id, name, email)

**Ejemplo body**

```json
{
  "id": "EP10",
  "name": "Emmanuel Perez",
  "email": "emmanuelp0918@gmail.com"
}
```


---

### 2. Listar clientes

* **URL:** `GET http://localhost:8080/api/bank/customers`

---

### 3. Buscar cliente por ID

* **URL:** `GET http://localhost:8080/api/bank/customers/{customerId}`
* **Ejemplo:** `GET http://localhost:8080/api/bank/customers/EP10 `

---

### 4. Crear una cuenta para un cliente ya sea de ahorros o corriente

* **URL:** `POST http://localhost:8080/api/bank/customers/{customerId}/accounts`
* **Path param:** `customerId` (string)
* **Body:** `AccountCreationRequest` (JSON):

  * `type` — `SAVINGS` o `CHECKING`
  * `accountId` — id deseada para la cuenta
  * `parameter` — para `SAVINGS` es la tasa de interés (ej. `0.02` = 2%). Para `CHECKING` es `overdraftLimit`.

**Ejemplo para cuenta de ahorros**

```json
{
  "type": "Ahorros",
  "accountId": "ACC_EP10",
  "parameter": 0.02
}
```

**Ejemplo para cuenta corriente**

```json
{
  "type": "Corriente",
  "accountId": "CORR_EP10",
  "parameter": 500.0
}
```

---

### 5. Buscar cuenta por ID

* **URL:** `GET http://localhost:8080/api/bank/accounts/{accountId}`
* **Ejemplo:** `GET http://localhost:8080/api/bank/accounts/ACC_EP10`


---

### 6. Listar cuentas de un cliente

* **URL:** `GET http://localhost:8080/api/bank/customers/{customerId}/accounts`


---

### 7. Realizar un depósito

* **URL:** `POST http://localhost:8080/api/bank/accounts/{accountId}/deposit?amount={amount}`
* **Parámetro HTTP:** `amount` (double) como query param



---

### 8. Realizar un retiro

* **URL:** `POST http://localhost:8080/api/bank/accounts/{accountId}/withdraw?amount={amount}`
* **Parámetro:** `amount` (double)



---

### 9. Transferencia entre cuentas

* **URL:** `POST http://localhost:8080/api/bank/accounts/{fromAccountId}/transfer`
* **Body (JSON)** `TransferRequest`:

```json
{
  "toAccountId": "ACC_EP10",
  "amount": 75.0
}
```




---

### 10. Consultar transacciones de una cuenta

* **URL:** `GET  http://localhost:8080/api/bank/accounts/{accountId}/transactions`


---

### 11. Aplicar intereses (a cuenta de ahorro)

* **URL:** `POST  http://localhost:8080/api/bank/accounts/{accountId}/apply-interest`
* **Uso:** Llama a `applyInterest()` para la cuenta (aplica tasa). 


---

## Documentación automática con Swagger / OpenAPI


**Cómo usarlo**

1. Arranca la aplicación.
2. Abre la URL ` http://localhost:8080/swagger-ui.html` en el navegador. Verás la lista de endpoints, esquemas de modelos y podrás probar cada operación desde la UI.
3. Si quieres personalizar la URL base visible en Swagger o agregar descripciones más largas, edita `OpenApiConfig` y anota controladores o métodos con `@Operation(summary = "...", description = "...")` y `@ApiResponse(...)` para documentar respuestas.

---

CREACION DE UN CLIENTE
<img width="1263" height="609" alt="Captura de pantalla 2025-10-30 143132" src="https://github.com/user-attachments/assets/960188bb-a8c5-45c2-ba5d-eeec598e8e75" />

LISTADO DE CLIENTES
<img width="1258" height="616" alt="Captura de pantalla 2025-10-30 154654" src="https://github.com/user-attachments/assets/c57a2255-7905-42c7-87ca-f02040a5ac80" />

BUSQUEDA DE CLIENTE MEDIANTE ID
<img width="1113" height="604" alt="Captura de pantalla 2025-10-30 154933" src="https://github.com/user-attachments/assets/cce3f1d8-1418-4b00-bbcf-e89ff1f8a62f" />

CREAR CUENTA DE AHORROS-CORRIENTE
<img width="1120" height="583" alt="Captura de pantalla 2025-10-30 155210" src="https://github.com/user-attachments/assets/9d73b723-bea2-4a84-a8cd-15d8a4ed3acf" />

OPERACION (DEPOSITO)
<img width="1339" height="693" alt="Captura de pantalla 2025-10-30 155420" src="https://github.com/user-attachments/assets/c42ed481-196d-46f4-99da-cf77743deb19" />

OPERACION (RETIRO)
<img width="1081" height="613" alt="Captura de pantalla 2025-10-30 155632" src="https://github.com/user-attachments/assets/83df2e46-210b-4b51-98c5-a60af0e3a793" />

TRANSFERIR DINERO 
<img width="937" height="629" alt="Captura de pantalla 2025-10-30 155842" src="https://github.com/user-attachments/assets/10af448c-9ac7-4e0c-87fe-1b06a8de3edf" />

APLICACION DE INTERESES
<img width="951" height="595" alt="Captura de pantalla 2025-10-30 160113" src="https://github.com/user-attachments/assets/59fe1dc8-a6ea-45d3-8e71-29cfaadcf5d2" />



































## Ejemplos de flujo de pruebas 

1. Crear cliente `EP10`.
2. Crear cuenta de ahorro `ACC_EP10` para `EP10` con tasa `0.02`.
3. Hacer depósito de `200.0` en `ACC_EP10`.
4. Consultar `GET /accounts/sav-1001` → verificar balance.
5. Aplicar interés `POST /accounts/ACC_EP10/apply-interest` → consultar nuevamente el balance.
6. Crear cuenta corriente `CORR_EP10`, transferir entre cuentas y revisar transacciones.

---


