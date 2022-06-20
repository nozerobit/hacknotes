# PHP Type Juggling

## PHP Type Juggling

### PHP Type Juggling Examples

### strcmp

**strcmp** compares two data types, it is bad when it is used to compare user entries because it’s not taking care of the entry type:

```http
# Doesn't work
username=admin&password=admin 
# Works
username[]=admin&password[]=admin 
```

