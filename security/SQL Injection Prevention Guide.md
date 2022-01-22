# Best practices
https://snyk.io/blog/sql-injection-cheat-sheet/  
- Do not rely on client-side input validation
- Use a database user with restricted privileges
- Use prepared statements and query parameterization
- Scan your code for SQL injection vulnerabilities
- Use an ORM layer
- Don’t rely on blocklisting
- Perform input validation
- Be careful with stored procedures  
Like SQL queries in your application, you should parameterize the queries in your stored procedure rather than concatenate the parameters. SQL injection in a stored procedure is quite easy to prevent.

## Secure code review: 8 security code review best practices
https://snyk.io/blog/secure-code-review/
- Sanitize and validate all input  
Basically every input that comes from the outside boundaries of your system should be considered and treated as potentially harmful. Think about things like:  
  - data feeds
  - files
  - events — an event-driven system, such as working closely with platforms like Functions as a Service
  - data responses from other systems
  - cookies

- Never store secrets as code/config  
- Test for new security vulnerabilities introduced by third-party open source dependencies
- Enforce secure authentication  
- Enforce the least privilege principle
- Handle sensitive data with care
- Protect against well-known attacks
- Statically test your source code, automatically

## Query Parameterization Cheat Sheet
https://cheatsheetseries.owasp.org/cheatsheets/Query_Parameterization_Cheat_Sheet.html

## SQL Injection Prevention Cheat Sheet
https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
