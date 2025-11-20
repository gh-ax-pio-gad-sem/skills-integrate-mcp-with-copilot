# Authentication System Documentation

## Overview

This application now includes a secure authentication and authorization system with role-based access control. Only authenticated teachers and administrators can register or unregister students from activities.

## Features Implemented

### 1. User Authentication
- **JWT Token-based authentication** using industry-standard libraries
- **Bcrypt password hashing** for secure password storage
- **Session management** with 8-hour token expiration
- **Persistent login** using localStorage

### 2. Role-Based Authorization
- **Teacher role**: Can manage student registrations and view activities
- **Admin role**: Full access to all features
- **Unauthenticated users**: Can only view activities (read-only access)

### 3. Security Features
- Passwords are hashed using bcrypt before storage
- JWT tokens for secure session management
- HTTP Bearer token authentication
- Protected API endpoints requiring authentication
- Authorization checks on sensitive operations

### 4. User Interface
- Login modal for teacher/admin authentication
- User info display showing logged-in user's name and role
- Logout functionality
- Registration and unregistration buttons only visible to authenticated users

## Default User Accounts

The following test accounts are pre-configured in `users.json`:

| Username  | Password | Role    | Full Name      |
|-----------|----------|---------|----------------|
| admin     | password | admin   | Administrator  |
| teacher1  | password | teacher | Teacher One    |
| teacher2  | password | teacher | Teacher Two    |

**⚠️ IMPORTANT**: These are test credentials. In production, change the passwords and update the `SECRET_KEY` in `app.py`.

## How to Use

### For Teachers/Admins:
1. Click the **Login** button in the top-right corner
2. Enter your username and password
3. Once logged in, you can:
   - Sign up students for activities
   - Remove students from activities
   - View all participants

### For Students/Public:
- View all available activities
- See participant lists
- Cannot modify registrations (must contact a teacher)

## API Endpoints

### Authentication Endpoints
- `POST /auth/login` - Authenticate and receive JWT token
  - Parameters: `username`, `password`
  - Returns: `access_token`, `token_type`, `user` info

- `GET /auth/me` - Get current user information
  - Requires: Bearer token in Authorization header
  - Returns: Current user details or null if not authenticated

### Activity Endpoints (Protected)
- `POST /activities/{activity_name}/signup` - Register a student (teachers only)
  - Requires: Bearer token
  - Parameters: `email`

- `DELETE /activities/{activity_name}/unregister` - Unregister a student (teachers only)
  - Requires: Bearer token
  - Parameters: `email`

### Public Endpoints
- `GET /activities` - View all activities (no authentication required)

## Technical Implementation

### Backend (Python/FastAPI)
- **FastAPI** framework for API endpoints
- **python-jose** for JWT token creation and validation
- **passlib** with bcrypt for password hashing
- **HTTPBearer** security scheme for token-based authentication
- Dependency injection for authentication/authorization checks

### Frontend (JavaScript)
- JWT token stored in localStorage
- Authorization header added to all authenticated requests
- Dynamic UI updates based on authentication status
- Modal-based login interface

## Security Considerations

### Current Implementation
- ✅ Passwords hashed with bcrypt
- ✅ JWT tokens for session management
- ✅ Role-based access control
- ✅ Protected API endpoints

### Production Recommendations
1. **Change SECRET_KEY**: Update the secret key in `app.py` to a strong, random value
2. **Update Passwords**: Change all default passwords in `users.json`
3. **Use HTTPS**: Deploy with SSL/TLS encryption
4. **Add Rate Limiting**: Prevent brute force login attempts
5. **Password Strength**: Implement password complexity requirements
6. **Database Migration**: Move from JSON file to proper database (see issue #8)
7. **Password Reset**: Implement forgot password functionality
8. **Account Management**: Add user registration and profile management

## Files Modified/Created

- ✅ `requirements.txt` - Added authentication dependencies
- ✅ `src/users.json` - User credentials storage (NEW)
- ✅ `src/app.py` - Authentication endpoints and middleware
- ✅ `src/static/index.html` - Login UI components
- ✅ `src/static/app.js` - Authentication logic
- ✅ `src/static/styles.css` - Login modal styling

## Related Issues

This implementation addresses:
- Issue #12: Implement user authentication and authorization system
- Issue #5: Admin Mode (partially - provides more comprehensive solution)

## Future Enhancements

Consider implementing:
- User registration workflow
- Password reset functionality
- Multi-factor authentication
- Audit logging for all actions
- Integration with school's existing authentication system (LDAP/SSO)
