# fielder
A go generate utility to generate fields related functions for struct.
"I just want some fields of the struct", this req is so common to me, and I don't want to use reflect stuff just on this, so copied from stringer and modified it to generate helper functions.

# Install
go install github.com/shuoli84/fielder

# Usage
```go
//go generate fielder -type=TestFielder -tagName=fielder
// You can also use -tagName=json to reuse json tags.
type TestFielder struct {
    StringField string `fielder:"string_field"`
    IntField int32 `fielder:"int_field"`
}
```

=>

```go
// Will generate code like following:

// FieldRefForName: Return the reference to the field
func (r *TestFielder) FieldRefForName(name string) (interface{}, error) {
    switch name {
    case "string_field":
        return &r.StringField, nil
    case "int_field":
        return &r.IntField, nil
    default:
        ...
    }
}

// FieldForName: Return the value to the field
func (r *TestFielder) FieldForName(name string) (interface{}, error) {
    switch name {
    case "string_field":
        return r.StringField, nil
    case "int_field":
        return r.IntField, nil
    default:
        ...
    }
}

// MapForFields: Return a map only contains fields with passed in
func (r *TestFielder) MapForFields(fieldNames []string) (map[string]interface{}, error) {
    resultMap := map[string]interface{}{}

    for _, fieldName := range fieldNames {
        switch fieldName {
        case "string_field":
            resultMap["string_field"] = r.StringField
        ...
        }
    }

    return resultMap, nil
}
```

