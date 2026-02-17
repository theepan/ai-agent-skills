# Java Security Review Checklist

Quick reference for OWASP-aligned Java security review.

## Table of Contents
1. [Injection](#injection)
2. [Authentication](#authentication)
3. [Sensitive Data](#sensitive-data)
4. [XML/Deserialization](#xmldeserialization)
5. [Access Control](#access-control)
6. [Cryptography](#cryptography)

## Injection

### SQL Injection
```java
// VULNERABLE
String query = "SELECT * FROM users WHERE id = " + userId;
stmt.executeQuery(query);

// SAFE - Parameterized
PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
ps.setString(1, userId);
```

**Check for:**
- String concatenation in SQL queries
- `Statement` instead of `PreparedStatement`
- Dynamic table/column names (use allowlist validation)
- JPA/Hibernate: `createQuery` with string concat vs `setParameter`

### Command Injection
```java
// VULNERABLE
Runtime.getRuntime().exec("cmd /c " + userInput);

// SAFE - Use ProcessBuilder with array
ProcessBuilder pb = new ProcessBuilder("cmd", "/c", "dir", safeArg);
```

### LDAP Injection
```java
// VULNERABLE
String filter = "(uid=" + username + ")";

// SAFE - Escape special characters
String safeUser = LdapEncoder.filterEncode(username);
```

## Authentication

**Check for:**
- Hardcoded credentials: `password = "admin123"`
- Credentials in logs: `log.info("User {} password {}", user, pass)`
- Timing attacks in password comparison (use `MessageDigest.isEqual`)
- Session fixation: regenerate session ID after login
- Missing brute-force protection

## Sensitive Data

### Logging
```java
// VULNERABLE - PII in logs
logger.info("Processing payment for card: " + cardNumber);

// SAFE - Mask sensitive data
logger.info("Processing payment for card: ****{}", last4Digits);
```

### Memory
```java
// VULNERABLE - String holds password (immutable, stays in memory)
String password = getPassword();

// SAFER - char[] can be zeroed
char[] password = getPassword();
Arrays.fill(password, '0');  // Clear after use
```

## XML/Deserialization

### XXE (XML External Entity)
```java
// VULNERABLE
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.parse(xmlInput);

// SAFE - Disable external entities
dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
dbf.setFeature("http://xml.org/sax/features/external-general-entities", false);
dbf.setFeature("http://xml.org/sax/features/external-parameter-entities", false);
```

### Unsafe Deserialization
```java
// VULNERABLE - Arbitrary object deserialization
ObjectInputStream ois = new ObjectInputStream(untrustedInput);
Object obj = ois.readObject();

// SAFE - Use allowlist filter (Java 9+)
ObjectInputFilter filter = ObjectInputFilter.Config.createFilter("com.myapp.*;!*");
ois.setObjectInputFilter(filter);
```

## Access Control

**Check for:**
- Missing `@PreAuthorize` / `@Secured` on sensitive endpoints
- Direct object references without ownership check
- Role checks only at UI level, not service layer
- Path traversal: `new File(baseDir + userInput)` -> use `Path.normalize()`

```java
// VULNERABLE - No ownership check
public Order getOrder(Long orderId) {
    return orderRepo.findById(orderId);
}

// SAFE - Verify ownership
public Order getOrder(Long orderId, Long userId) {
    Order order = orderRepo.findById(orderId);
    if (!order.getUserId().equals(userId)) throw new AccessDeniedException();
    return order;
}
```

## Cryptography

**Red flags:**
- `MD5`, `SHA1` for passwords (use bcrypt/Argon2)
- `DES`, `3DES`, `RC4` (use AES-256-GCM)
- `ECB` mode (use GCM or CBC with HMAC)
- Hardcoded keys/IVs
- `SecureRandom` not used for crypto operations
- Missing certificate validation in HTTPS

```java
// VULNERABLE
Cipher.getInstance("AES/ECB/PKCS5Padding");
MessageDigest.getInstance("MD5");

// SAFE
Cipher.getInstance("AES/GCM/NoPadding");
// For passwords: BCrypt.hashpw(password, BCrypt.gensalt(12));
```
