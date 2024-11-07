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