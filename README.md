##Install
```bash
pip install git+https://github.com/npbridge/jsoncompare
```

##Explanation
*  Being able to tell whether or not an arbitrarily nested json blob is the same as another blob can hurt your eyes.
*  Additional complexity comes in when your json array's order doesn't matter.
*  For example, you might have a set in Java that you are going to send over the wire as json. It may be represented as a json array but in actuality, you don't care about the order.

## Recent Changes
    Version    Comments
    0.1.1      Orginal Version
    0.1.2      Adds support for "contains"

##Examples
```python
from jsoncompare import jsoncompare

# Compare respecting each array's order
jsoncompare.are_same(a, b)

# Compare ignoring each array's order
jsoncompare.are_same(a, b, True)

# Compare ignoring the value of certain keys
jsoncompare.are_same(a, b, False, ["datetime", "snacktime"])

# Contains at least
jsoncompare.contains(a, b)

```

```python
# Getting difference traces
from jsoncompare import jsoncompare

a = {
    "failureReason" : "Invalid request entity",
    "fieldValidationErrors" : [
        {
            "field" : "normal value 1",
            "reason" : "may not be smelly"
        },
        {
            "field" : "Catalog.name",
            "reason" : "may not be null"
        }
    ]
}
b = {
    "failureReason" : "Invalid request entity",
    "fieldValidationErrors" : [
        {
            "field" : "crazy value 2",
            "reason" : "may not be null"
        },
        {
            "field" : "Catalog.catalogOwner",
            "reason" : "may not be null"
        }
    ]
}
print jsoncompare.are_same(a, b)[1]
```
results in:

```python
Reason: Different values
Expected:
  "normal value 1"
Actual:
  "crazy value 2"

Reason: Different values (Check order)
Expected:
  {
      "field": "normal value 1", 
      "reason": "may not be smelly"
  }
Actual:
  {
      "field": "crazy value 2", 
      "reason": "may not be null"
  }

Reason: Different values
Expected:
  [
      {
          "field": "normal value 1", 
          "reason": "may not be smelly"
      }, 
      {
          "field": "Catalog.name", 
          "reason": "may not be null"
      }
  ]
Actual:
  [
      {
          "field": "crazy value 2", 
          "reason": "may not be null"
      }, 
      {
          "field": "Catalog.catalogOwner", 
          "reason": "may not be null"
      }
  ]
```
