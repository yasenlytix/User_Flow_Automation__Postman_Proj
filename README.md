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


## ***❶ Login (extract token)***

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

#### Under body section:-
```js
{
    "email": "eve.holt@reqres.in",
    "password": "cityslicka"
}
```

<br>

#### Under Post-request Script:-

```js
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

<br>

#### Under Headers:
```
"x-api-key": "put value of your api key"
```

<br>

#### Under Authorization:

➥ Auth Type ---> `Bearer Token`

➥ Token ---> `{{token}}`

<br>
<br>
<br>


## ***❷ Create User***

<br>

#### Request URL:

> `POST` {{baseUrl}}/api/users

<br>

#### Under body section:-
```js
{
    "name": "Muhammad Yaseen",
    "job": "SQA Engineer",
    "city": "Lahore"
}
```

<br>

#### Under Post-request Script:-
```js
const jsonData = pm.response.json();

// Creating userId environment
pm.environment.set("userId", jsonData.id)

// testing userId is stored
pm.test("userId is stored", () => {
    pm.expect(jsonData.id).to.not.be.undefined;
    console.log(jsonData.id);
});
```

<br>

#### Under Headers:
```
"x-api-key": "put value of your api key"
```

<br>

#### Under Authorization:

➥ Auth Type ---> `Bearer Token`

➥ Token ---> `{{token}}`

<br>
<br>
<br>

## ***❸ Update User***

<br>

#### Request URL:

> `PUT`  {{baseUrl}}/api/users/{{userId}}

<br>

#### Under body section:-
```js
{
    "name": "Yaseen Mahi",
    "job": "Software Quality Assurance Engineer",
    "city": "Sialkot"
}
```

<br>

#### Under Post-request Script:-
```js
// testing status-code
pm.test("Status code is 200", () => {
    pm.response.to.have.status(200);
});
```

<br>

#### Under Headers:
```
"x-api-key": "put value of your api key"
```

<br>

#### Under Authorization:

➥ Auth Type ---> `Bearer Token`

➥ Token ---> `{{token}}`

<br>
<br>
<br>

## ***❹ Get All Users***

<br>

#### Request URL:

> `GET`  {{baseUrl}}/api/users

<br>

#### Under Post-request Script:-
```js
// testing the response status code
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

<br>

#### Under Headers:
```
"x-api-key": "put value of your api key"
```

<br>

#### Under Authorization:

➥ Auth Type ---> `Bearer Token`

➥ Token ---> `{{token}}`

<br>
<br>
<br>

## ***❺ Delete User***

<br>

#### Request URL:

> `DELETE`  {{baseUrl}}/api/users/{{userId}}

<br>

#### Under Post-request Script:-
```js
// Testing response status code is 204 (No Content)
pm.test("Status code is 204", () => {
    pm.response.to.have.status(204);
});

// Deleting all environment variables
pm.environment.unset("userId")
pm.environment.unset("token")
pm.environment.unset("baseUrl")
```

<br>

#### Under Headers:
```
"x-api-key": "put value of your api key"
```

<br>

#### Under Authorization:

➥ Auth Type ---> `Bearer Token`

➥ Token ---> `{{token}}`

<br>


#### Note:-
> At the end, all the environment variables will be deleted!


---

<br>





