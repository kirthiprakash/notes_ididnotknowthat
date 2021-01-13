# Check nil interface value

{% embed url="https://mangatmodi.medium.com/go-check-nil-interface-the-right-way-d142776edef1" %}

```text
func isNil(i interface{}) bool {                        
   return i == nil || reflect.ValueOf(i).IsNil()                       }
```

