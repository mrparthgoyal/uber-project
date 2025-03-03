# API Documentation

## POST /users/register

### Description
This endpoint is used to register a new user.

### Request Body
The request body must be a JSON object containing the following fields:
- `fullname`: An object containing:
  - `firstname`: A string with at least 3 characters (required)
  - `lastname`: A string with at least 3 characters (optional)
- `email`: A valid email address (required)
- `password`: A string with at least 6 characters (required)

### Example Request
```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "password123"
}
```

### Responses

#### Success
- **Status Code**: 201 Created
- **Response Body**:
  ```json
  {
    "token": "jwt-token",
    "user": {
      "_id": "user-id",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

#### Validation Errors
- **Status Code**: 400 Bad Request
- **Response Body**:
  ```json
  {
    "errors": [
      {
        "msg": "Invalid Email",
        "param": "email",
        "location": "body"
      },
      {
        "msg": "First name must be at least 3 characters long",
        "param": "fullname.firstname",
        "location": "body"
      },
      {
        "msg": "Password must be at least 6 characters long",
        "param": "password",
        "location": "body"
      }
    ]
  }
  ```

### Notes
- Ensure that the `Content-Type` header is set to `application/json` when making requests to this endpoint.

## POST /users/login

### Description
This endpoint is used to log in an existing user.

### Request Body
The request body must be a JSON object containing the following fields:
- `email`: A valid email address (required)
- `password`: A string with at least 6 characters (required)

### Example Request
```json
{
  "email": "john.doe@example.com",
  "password": "password123"
}
```

### Responses

#### Success
- **Status Code**: 200 OK
- **Response Body**:
  ```json
  {
    "token": "jwt-token",
    "user": {
      "_id": "user-id",
      "fullname": {
        "firstname": "John",
        "lastname": "Doe"
      },
      "email": "john.doe@example.com"
    }
  }
  ```

#### Validation Errors
- **Status Code**: 400 Bad Request
- **Response Body**:
  ```json
  {
    "errors": [
      {
        "msg": "Invalid Email",
        "param": "email",
        "location": "body"
      },
      {
        "msg": "Password must be at least 6 characters long",
        "param": "password",
        "location": "body"
      }
    ]
  }
  ```

#### Authentication Errors
- **Status Code**: 401 Unauthorized
- **Response Body**:
  ```json
  {
    "message": "Invalid name or password"
  }
  ```

### Notes
- Ensure that the `Content-Type` header is set to `application/json` when making requests to this endpoint.

## GET /users/profile

### Description
This endpoint retrieves the profile information of the currently authenticated user.

### Headers
- `Authorization`: Bearer token (JWT) required

### Response

#### Success
- **Status Code**: 200 OK
- **Response Body**:
  ```json
  {
    "_id": "user-id",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
  ```

#### Authentication Error
- **Status Code**: 401 Unauthorized
- **Response Body**:
  ```json
  {
    "message": "Authentication required"
  }
  ```

## GET /users/logout

### Description
This endpoint logs out the current user by invalidating their token.

### Headers
- `Authorization`: Bearer token (JWT) required

### Response

#### Success
- **Status Code**: 200 OK
- **Response Body**:
  ```json
  {
    "message": "Logged out"
  }
  ```

#### Authentication Error
- **Status Code**: 401 Unauthorized
- **Response Body**:
  ```json
  {
    "message": "Authentication required"
  }
  ```

### Notes
- The token will be blacklisted and can no longer be used for authentication
- The session cookie will be cleared if it exists

## POST /captains/register

### Description
This endpoint is used to register a new captain.

### Request Body
The request body must be a JSON object containing the following fields:
- `fullname`: An object containing:
  - `firstname`: A string with at least 3 characters (required)
  - `lastname`: A string (optional)
- `email`: A valid email address (required)
- `password`: A string with at least 6 characters (required)
- `vehicle`: An object containing:
  - `color`: A string with at least 3 characters (required)
  - `plate`: A string with at least 3 characters (required)
  - `capacity`: A number (required)
  - `vehicleType`: One of: 'car', 'motorcycle', 'auto' (required)

### Example Request
```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Smith"
  },
  "email": "john.smith@example.com",
  "password": "password123",
  "vehicle": {
    "color": "black",
    "plate": "ABC123",
    "capacity": 4,
    "vehicleType": "car"
  }
}
```

### Responses

#### Success
- **Status Code**: 201 Created
- **Response Body**:
  ```json
  {
    "token": "jwt-token",
    "captain": {
      "_id": "captain-id",
      "fullname": {
        "firstname": "John",
        "lastname": "Smith"
      },
      "email": "john.smith@example.com",
      "vehicle": {
        "color": "black",
        "plate": "ABC123",
        "capacity": 4,
        "vehicleType": "car"
      }
    }
  }
  ```

#### Validation Errors
- **Status Code**: 400 Bad Request
- **Response Body**:
  ```json
  {
    "errors": [
      {
        "msg": "Invalid Email",
        "param": "email",
        "location": "body"
      },
      {
        "msg": "First name must be at least 3 characters long",
        "param": "fullname.firstname",
        "location": "body"
      },
      {
        "msg": "Vehicle type must be one of: car, motorcycle, auto",
        "param": "vehicle.vehicleType",
        "location": "body"
      }
    ]
  }
  ```

### Notes
- Ensure that the `Content-Type` header is set to `application/json` when making requests to this endpoint.
- All vehicle information is required for captain registration.

## POST /captains/login

### Description
This endpoint is used to log in an existing captain.

### Request Body
The request body must be a JSON object containing the following fields:
- `email`: A valid email address (required)
- `password`: A string with at least 6 characters (required)

### Example Request
```json
{
  "email": "john.smith@example.com",
  "password": "password123"
}
```

### Responses

#### Success
- **Status Code**: 200 OK
- **Response Body**:
  ```json
  {
    "token": "jwt-token",
    "captain": {
      "_id": "captain-id",
      "fullname": {
        "firstname": "John",
        "lastname": "Smith"
      },
      "email": "john.smith@example.com",
      "vehicle": {
        "color": "black",
        "plate": "ABC123",
        "capacity": 4,
        "vehicleType": "car"
      }
    }
  }
  ```

#### Validation Errors
- **Status Code**: 400 Bad Request
- **Response Body**:
  ```json
  {
    "errors": [
      {
        "msg": "Invalid Email",
        "param": "email",
        "location": "body"
      },
      {
        "msg": "Password must be at least 6 characters long",
        "param": "password",
        "location": "body"
      }
    ]
  }
  ```

#### Authentication Errors
- **Status Code**: 401 Unauthorized
- **Response Body**:
  ```json
  {
    "message": "Invalid email or password"
  }
  ```

## GET /captains/profile

### Description
This endpoint retrieves the profile information of the currently authenticated captain.

### Headers
- `Authorization`: Bearer token (JWT) required

### Response

#### Success
- **Status Code**: 200 OK
- **Response Body**:
  ```json
  {
    "_id": "captain-id",
    "fullname": {
      "firstname": "John",
      "lastname": "Smith"
    },
    "email": "john.smith@example.com",
    "vehicle": {
      "color": "black",
      "plate": "ABC123",
      "capacity": 4,
      "vehicleType": "car"
    }
  }
  ```

#### Authentication Error
- **Status Code**: 401 Unauthorized
- **Response Body**:
  ```json
  {
    "message": "Authentication required"
  }
  ```

## GET /captains/logout

### Description
This endpoint logs out the current captain by invalidating their token.

### Headers
- `Authorization`: Bearer token (JWT) required

### Response

#### Success
- **Status Code**: 200 OK
- **Response Body**:
  ```json
  {
    "message": "Logout successfully"
  }
  ```

#### Authentication Error
- **Status Code**: 401 Unauthorized
- **Response Body**:
  ```json
  {
    "message": "Authentication required"
  }
  ```

### Notes
- The token will be blacklisted and can no longer be used for authentication
- The session cookie will be cleared if it exists
