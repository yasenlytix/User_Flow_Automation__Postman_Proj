# **Automating API Test Flow with Postman Collections & Runner**

<br>

# **😵 Scenario Overview**

The following essentials are mandatory.

<br>

## ***❶ A simple User Management API Backend System***

<br>

## ***❷ Test Flow***

<div align="center">
  <img src="https://github.com/yasenlytix/User_Flow_Automation__Postman_Proj/blob/main/postman_api_test_flow.png" alt="Alt Text" width="500"/>
</div>

<br>

## ***❸ I will run all these steps as one automated flow using ---> Postman Collection Runner***

<br>

---

<br>
<br>
<br>


## **🤓 Deliverables**

<br>

➥ Postman Collection with at least 5 requests.

➥ Automated creation & utilization of **Environment variables** for **base URL** & **dynamic variables**.

➥ Automated run report (via Postman Collection Runner).

<br>
<br>


## **🤓 End Result**

<br>

➥ A **complete API workflow** in one go.

➥ Variables will be **dynamically passed**, and **request chaining** will be **automated**.

<br>

---

<br>
<br>
<br>


## Login (extract token)

<br>

## **Step 1**

I created a new environment inside Postman.

➥ `VarDataEnv`

<br>

## **Step 2**


I wrote JavaScript under `pre-request Script` to add an environment variable dynamically

```js
pm.environment.set("baseUrl", "https://reqres.in");
```

**Hence:-**
> baseUrl ---> https://reqres.in

<br>

## **Step 3**

**I created `POST` request:-**

> {{baseUrl}}/api/login

<br>

**Under body section:-**
```js
{
    "email": "eve.holt@reqres.in",
    "password": "cityslicka"
}
```

<br>
<br>

**Post-request Script:-**

```json
const jsonData = pm.response.json();

// Capturing token variable
pm.environment.set("token", jsonData.token);

// testing token is received
pm.test("Token is received", () => {
    pm.expect(jsonData.token).to.not.be.undefined;
    console.log("token value: ", jsonData.token);
    console.log("Environment token value: ", pm.environment.get("token"));
});

// testing environment-token & response-token are same
pm.test("Token in environment matches response token", () => {
    pm.expect(jsonData.token).to.eql(pm.environment.get("token"));
});
```
