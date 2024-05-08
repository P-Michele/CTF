# Preventing Path Traversal Vulnerabilities

## Introduction

Path traversal vulnerabilities occur when an application uses user input to access file system resources without properly sanitizing the input. This can allow attackers to access files and directories stored outside the intended directories.

## Best Practices for Prevention

### Input Whitelisting

- **Strict Whitelisting**: Always validate user inputs against a whitelist of permitted values. If the application functionality requires flexible inputs, ensure they are strictly alphanumeric.

### Canonicalizing Paths

- **Path Canonicalization**: After appending user input to a base directory, use platform-specific filesystem APIs to resolve the absolute canonical path and ensure it begins with the expected base directory.

```
File file = new File(BASE_DIRECTORY, userInput);
if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) {
    // Process file safely
}

```
# Common Path Traversal Payloads
-Windows and Unix Variants

    Traversal Sequences:
        For Windows: ../ or ..\
        For Unix/Linux: ../

-Absolute Paths

    Direct File Access:
        Example: filename=/etc/passwd
        This approach attempts to directly access a file without using traversal sequences.

-Nested and Encoded Sequences

    Nested Traversal:
        Example: ....// or ....\/
        Use when simple traversal is stripped, as inner sequences revert to standard traversal upon stripping.

    URL Encoding:
        Single URL encode: %2e%2e%2f (represents ../)
        Double URL encode: %252e%252e%252f (prevents simple URL decode from neutralizing traversal)
        Non-standard encodings: ..%c0%af or ..%ef%bc%8f

-Special Cases

    Starting with Base Folder:
        Example: /var/www/images/../../..
        Exploits applications that only check if the path starts with a specified base folder but don't verify the completeness of the path.

-Null Byte Injection

    File Extension Manipulation:
        Example: filename=../../../etc/passwd%00.png
        Useful in contexts where file extensions are required by the application, null byte %00 can terminate the string early in some environments.
