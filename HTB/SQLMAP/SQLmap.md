| Target connection          | Injection detection | Fingerprinting                                         |
| -------------------------- | ------------------- | ------------------------------------------------------ |
| Enumeration                | Optimization        | Protection detection and bypass using "tamper" scripts |
| Database content retrieval | File system access  | Execution of the operating system (OS) commands        |

## Boolean-based blind SQL Injection

Example of `Boolean-based blind SQL Injection`:

Code: sql

```sql
AND 1=1
```

SQLMap exploits `Boolean-based blind SQL Injection` vulnerabilities through the differentiation of `TRUE` from `FALSE` query results, effectively retrieving 1 byte of information per request. The differentiation is based on comparing server responses to determine whether the SQL query returned `TRUE` or `FALSE`. This ranges from fuzzy comparisons of raw response content, HTTP codes, page titles, filtered text, and other factors.

- `TRUE` results are generally based on responses having none or marginal difference to the regular server response.
    
- `FALSE` results are based on responses having substantial differences from the regular server response.
    
- `Boolean-based blind SQL Injection` is considered as the most common SQLi type in web applications.
    

---

## Error-based SQL Injection

Example of `Error-based SQL Injection`:

Code: sql

```sql
AND GTID_SUBSET(@@version,0)
```

If the `database management system` (`DBMS`) errors are being returned as part of the server response for any database-related problems, then there is a probability that they can be used to carry the results for requested queries. In such cases, specialized payloads for the current DBMS are used, targeting the functions that cause known misbehaviors. SQLMap has the most comprehensive list of such related payloads and covers `Error-based SQL Injection` for the following DBMSes:

||||
|---|---|---|