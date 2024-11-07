## **Meal Plan Management**

### **1. Create a New Meal Plan**

- **Endpoint:** `POST /api/meal-plans`
- **Description:** Allows authenticated users to create a new meal plan by specifying details like name, description, start date, and end date.
- **Process:**
    - Validates input data (e.g., non-empty name, valid date ranges).
    - Associates the meal plan with the current user.
    - Stores the meal plan in the database.

### **2. Retrieve All Meal Plans**

- **Endpoint:** `GET /api/meal-plans`
- **Description:** Fetches all meal plans associated with the currently authenticated user.
- **Process:**
    - Retrieves user ID from the JWT token.
    - Queries the database for all meal plans linked to the user.
    - Returns a list of `MealPlanDTO` objects.

### **3. Retrieve a Specific Meal Plan**

- **Endpoint:** `GET /api/meal-plans/{id}`
- **Description:** Retrieves detailed information about a specific meal plan by its ID.
- **Process:**
    - Validates that the meal plan exists and belongs to the user.
    - Returns the meal plan details as a `MealPlanDTO`.

### **4. Update a Meal Plan**

- **Endpoint:** `PUT /api/meal-plans/{id}`
- **Description:** Allows users to update details of an existing meal plan.
- **Process:**
    - Validates input data and ensures the meal plan belongs to the user.
    - Updates meal plan attributes in the database.
    - Returns the updated `MealPlanDTO`.

### **5. Delete a Meal Plan**

- **Endpoint:** `DELETE /api/meal-plans/{id}`
- **Description:** Enables users to delete an existing meal plan.
- **Process:**
    - Validates that the meal plan exists and belongs to the user.
    - Removes the meal plan and associated meals from the database.
    - Returns a `204 No Content` status upon successful deletion.

---

## **Meal Management within Meal Plans**

### **1. Add a Meal to a Meal Plan**

- **Endpoint:** `POST /api/meal-plans/{mealPlanId}/meals`
- **Description:** Adds a single meal to a specified meal plan.
- **Process:**
    - Validates input data (e.g., meal name, scheduled time).
    - Optionally links the meal to a recipe from the Edamam API using `recipeId`.
    - Saves the meal in the database and associates it with the meal plan.
    - Returns the added `MealDTO`.

### **2. Add Multiple Meals in Bulk**

- **Endpoint:** `POST /api/meal-plans/{mealPlanId}/meals/bulk`
- **Description:** Allows users to add multiple meals to a meal plan in a single request.
- **Process:**
    - Validates the list of meals provided.
    - Processes each meal, optionally linking to recipes.
    - Saves all meals in the database under the specified meal plan.
    - Returns a list of added `MealDTO` objects.

### **3. Update a Meal**

- **Endpoint:** `PUT /api/meal-plans/{mealPlanId}/meals/{mealId}`
- **Description:** Updates details of an existing meal within a meal plan.
- **Process:**
    - Validates that the meal exists and belongs to the specified meal plan and user.
    - Updates meal attributes in the database.
    - Returns the updated `MealDTO`.

### **4. Remove a Meal from a Meal Plan**

- **Endpoint:** `DELETE /api/meal-plans/{mealPlanId}/meals/{mealId}`
- **Description:** Deletes a specific meal from a meal plan.
- **Process:**
    - Validates that the meal exists and belongs to the specified meal plan and user.
    - Removes the meal from the database.
    - Returns a `204 No Content` status upon successful deletion.

---

## **Recipe Search and Retrieval**

### **1. Search for Recipes**

- **Endpoint:** `GET /api/recipes/search`
- **Description:** Searches for recipes based on various filters by interfacing with the Edamam API.
- **Supported Query Parameters:**
    - `q` (String): Search query (e.g., "chicken").
    - `beta` (Boolean): Use of beta features.
    - `diet` (List<String>): Dietary preferences (e.g., "low-carb").
    - `health` (List<String>): Health labels (e.g., "gluten-free").
    - `cuisineType` (List<String>): Types of cuisine (e.g., "Italian").
    - `mealType` (List<String>): Types of meals (e.g., "Dinner").
    - `dishType` (List<String>): Types of dishes (e.g., "Soup").
    - `excluded` (List<String>): Ingredients or items to exclude.
    - `co2EmissionsClass` (String): Environmental impact classification.
    - `field` (List<String>): Specific fields to retrieve.
- **Process:**
    - Validates that at least one relevant query parameter is provided.
    - Constructs a request to the Edamam API with the provided filters.
    - Returns a `RecipeSearchResponse` containing matching recipes

### **2. Retrieve a Specific Recipe**

- **Endpoint:** `GET /api/recipes/{id}`
- **Description:** Fetches detailed information about a specific recipe using its unique ID.
- **Process:**
    - Validates that the recipe ID is provided and formatted correctly.
    - Requests the recipe details from the Edamam API.
    - Returns the `Recipe` object containing detailed information.

---

## **User Roles and Permissions**

### **1. Role-Based Access Control (RBAC)**

- **Roles Defined:**
    - `ROLE_USER`: Default role assigned to all registered users.
    - `ROLE_ADMIN`: Elevated role with access to admin-specific functionalities.
- **Permissions:**
    - **Users (`ROLE_USER`):** Can manage their own meal plans and meals, search for recipes, and view their profiles.
    - **Admins (`ROLE_ADMIN`):** Can access admin endpoints, manage users, roles, and perform system-wide operations.

### **2. Assigning Roles**

- **During Registration:**
    - New users are automatically assigned the `ROLE_USER` role.
- **Role Management (Admin Functionality):**
    - Admins can assign or revoke roles for users through dedicated admin endpoints (to be implemented as needed).

---

## **Exception Handling and Validation**

### **1. Global Exception Handling**

- **Component:** `GlobalExceptionHandler`
- **Description:** Centralizes exception handling across all controllers, ensuring consistent and meaningful error responses.
- **Handled Exceptions:**
    - **Validation Errors:** `MethodArgumentNotValidException` returns detailed error messages for invalid input data.
    - **Resource Not Found:** `ResourceNotFoundException` returns `404 Not Found` with appropriate messages.
    - **Unauthorized Access:** `UnauthorizedException` returns `403 Forbidden`.
    - **Custom Exceptions:** Handles application-specific exceptions like `UsernameAlreadyExistsException`, `EmailAlreadyExistsException`, and `RecipeNotFoundException`.

### **2. Input Validation**

- **Annotations Used:**
    - `@NotBlank`, `@Size`, `@Email`, `@NotNull`, etc., ensure that incoming request data adheres to expected formats and constraints.
- **DTO Validation:**
    - DTOs like `UserDTO`, `MealDTO`, `MealPlanDTO`, and `BulkMealDTO` are annotated to enforce validation rules before processing.