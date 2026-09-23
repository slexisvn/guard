# slexisvn.guard

Composable runtime validation schemas for Tera data.

## Install

```bash
peta install slexisvn.guard
```

## Use

```tera
from slexisvn.guard import object, string, number, array, validate, format_issues

user_schema = object({
  name: string().min(2),
  email: string().email(),
  age: number().int().min(18),
  tags: array(string().min(1)).max(5),
}).strict()

result = validate(user_schema, {
  name: "Sinh",
  email: "sinh@example.com",
  age: 24,
  tags: ["tera", "petahub"],
})

if result.ok:
  print("valid user")
else:
  print(format_issues(result.issues))
```

## Schemas

- `any()`
- `string()`: `min`, `max`, `length`, `nonempty`, `includes`, `starts_with`, `ends_with`, `email`, `one_of`
- `number()`: `min`, `max`, `int`, `positive`, `nonnegative`, `multiple_of`
- `boolean()`: `true_only`, `false_only`
- `array(schema?)`: `of`, `min`, `max`, `length`, `nonempty`
- `object(shape)`: `strict`, `passthrough`, `extend`
- `literal(value)`
- `one_of(values)`
- `union([schema_a, schema_b])`
- `optional(schema)`, `nullable(schema)`, `nullish(schema)`

## Results

`validate(schema, value)` returns:

```tera
{
  ok: true,
  value: value,
  issues: [],
}
```

or:

```tera
{
  ok: false,
  value: null,
  issues: [
    {
      path: "email",
      code: "email",
      message: "email must be a valid email address",
      expected: "email",
      received: "bad-email",
    },
  ],
}
```

Use `parse(schema, value)` when you want invalid data to throw.
