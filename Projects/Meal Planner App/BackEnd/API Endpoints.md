elow is a structured summary of all possible API endpoints supported by your backend, along with their descriptions and access controls.

### **1. Authentication Endpoints**

|HTTP Method|Endpoint|Description|Access Control|
|---|---|---|---|
|POST|`/api/auth/register`|Register a new user|Public|
|POST|`/api/auth/login`|Authenticate user and issue JWT|Public|

### **2. User Endpoints**

|HTTP Method|Endpoint|Description|Access Control|
|---|---|---|---|
|GET|`/api/user/me`|Retrieve current user's profile|Authenticated Users|
|GET|`/api/user/admin`|Access admin-specific functionalities|`ROLE_ADMIN` Only|

### **3. Meal Plan Endpoints**

|HTTP Method|Endpoint|Description|Access Control|
|---|---|---|---|
|POST|`/api/meal-plans`|Create a new meal plan|Authenticated Users|
|GET|`/api/meal-plans`|Retrieve all meal plans for user|Authenticated Users|
|GET|`/api/meal-plans/{id}`|Retrieve a specific meal plan by ID|Authenticated Users|
|PUT|`/api/meal-plans/{id}`|Update a specific meal plan|Authenticated Users|
|DELETE|`/api/meal-plans/{id}`|Delete a specific meal plan|Authenticated Users|
|POST|`/api/meal-plans/{mealPlanId}/meals`|Add a meal to a meal plan|Authenticated Users|
|POST|`/api/meal-plans/{mealPlanId}/meals/bulk`|Add multiple meals to a meal plan|Authenticated Users|
|PUT|`/api/meal-plans/{mealPlanId}/meals/{mealId}`|Update a specific meal|Authenticated Users|
|DELETE|`/api/meal-plans/{mealPlanId}/meals/{mealId}`|Delete a specific meal|Authenticated Users|

### **4. Recipe Endpoints**

|HTTP Method|Endpoint|Description|Access Control|
|---|---|---|---|
|GET|`/api/recipes/search`|Search for recipes with filters|Authenticated Users|
|GET|`/api/recipes/{id}`|Retrieve a specific recipe by ID|Authenticated Users|