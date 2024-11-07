## **Data Persistence and Relationships**

### **1. Entities and Relationships**

- **User Entity:**
    - Fields: `id`, `username`, `password`, `email`, `roles`.
    - Relationships: Many-to-Many with `Role`.
- **Role Entity:**
    - Fields: `id`, `name`.
- **MealPlan Entity:**
    - Fields: `id`, `name`, `description`, `startDate`, `endDate`, `user`, `meals`.
    - Relationships: Many-to-One with `User`, One-to-Many with `Meal`.
- **Meal Entity:**
    - Fields: `id`, `name`, `recipeId`, `recipeName`, `recipeUrl`, `scheduledAt`, `mealPlan`.
    - Relationships: Many-to-One with `MealPlan`.

### **2. Database Operations**

- **Repositories:**
    
    - **UserRepository:** CRUD operations for `User` entities, plus methods to find by username and check existence.
    - **RoleRepository:** CRUD operations for `Role` entities, plus methods to find by name.
    - **MealPlanRepository:** CRUD operations for `MealPlan` entities, plus methods to find meal plans by user.
    - **MealRepository:** CRUD operations for `Meal` entities, plus methods to find meals by meal plan.
- **Cascade and Orphan Removal:**
    
    - **MealPlan to Meal:** Cascading ensures that when a meal plan is deleted, associated meals are also removed (`orphanRemoval = true`).