# json serialization

For primitive datatypes, `json.dumps()` serializes the passed dictionary to JSON representation.

For non-primitive types, it takes a function which can tell how to serialize a non-primitive type.

```text
def my_serializer(obj):
    if isinstance(obj, datetime.datetime):
        return obj.isoformat()
json.dumps(some_obj, default=my_serializer, indent=2, sort_keys=True)
```

While in a interactive python shell, if we want to quickly try the `dumps` function we can just pass `str` as the overiding serialization function

```text
json.dumps(some_obj, default=str, indent=2, sort_keys=True)
```

