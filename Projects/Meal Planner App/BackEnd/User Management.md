## **User Authentication and Authorization**

### **1. User Registration**

- **Endpoint:** `POST /api/auth/register`
- **Description:** Allows new users to create an account by providing a username, password, and email.
- **Process:**
    - Validates input data (e.g., username uniqueness, password strength, valid email format).
    - Encrypts the user's password using BCrypt.
    - Assigns a default role (e.g., `ROLE_USER`) to the new user.
    - Stores the user information in the database.

### **2. User Login**

- **Endpoint:** `POST /api/auth/login`
- **Description:** Authenticates existing users and issues a JWT token for session management.
- **Process:**
    - Validates user credentials (username and password).
    - Upon successful authentication, generates a JWT token containing user details and roles.
    - Returns the JWT token to the client for use in subsequent requests.

### **3. JWT-Based Security**

- **Mechanism:**
    - **Authentication Filter:** `JwtRequestFilter` intercepts incoming requests, extracts the JWT token, validates it, and sets the authentication context.
    - **Authorization:** Ensures that only authenticated users can access protected endpoints.
    - **Role-Based Access Control:** Uses annotations like `@PreAuthorize` to restrict access to certain endpoints based on user roles (e.g., `ROLE_ADMIN`).

---

## **User Management**

### **1. Retrieve Current User Profile**

- **Endpoint:** `GET /api/user/me`
- **Description:** Fetches the profile information of the currently authenticated user.
- **Process:**
    - Extracts user details from the JWT token.
    - Retrieves user data from the database, including username, email, and roles.
    - Returns the user profile data as a `UserProfileDTO`.

### **Admin-Only Endpoints**

- **Example Endpoint:** `GET /api/user/admin`
- **Description:** Provides access to admin-specific functionalities or data.
- **Access Control:** Restricted to users with the `ROLE_ADMIN` role using the `@PreAuthorize("hasRole('ADMIN')")` annotation.

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