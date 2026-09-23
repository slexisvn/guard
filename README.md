# slexisvn.guard

Composable runtime validation schemas for Tera data.

## Install

```bash
peta install slexisvn.guard
```

## Use

```tera
from slexisvn.guard import guard

user_schema = guard.object({
  name: guard.string().min(2),
  email: guard.string().email(),
  age: guard.number().int().min(18),
  tags: guard.array(guard.string().min(1)).max(5),
}).strict()

result = guard.validate(user_schema, {
  name: "Sinh",
  email: "sinh@example.com",
  age: 24,
  tags: ["tera", "petahub"],
})

if result.ok:
  print("valid user")
else:
  print(guard.format_issues(result.issues))
```

## Schemas

- `guard.any()`
- `guard.string()`: `min`, `max`, `length`, `nonempty`, `includes`, `starts_with`, `ends_with`, `email`, `one_of`
- `guard.number()`: `min`, `max`, `int`, `positive`, `nonnegative`, `multiple_of`
- `guard.boolean()`: `true_only`, `false_only`
- `guard.array(schema?)`: `of`, `min`, `max`, `length`, `nonempty`
- `guard.object(shape)`: `strict`, `passthrough`, `extend`
- `guard.literal(value)`
- `guard.one_of(values)`
- `guard.union([schema_a, schema_b])`
- `guard.optional(schema)`, `guard.nullable(schema)`, `guard.nullish(schema)`

## Types

```tera
from slexisvn.guard import guard, GuardResultOf, StringGuard

schema: StringGuard = guard.string().min(1).email()
result: GuardResultOf<string> = schema.validate("sinh@example.com")
```

Public schema contracts: `GuardSchema`, `StringGuard`, `NumberGuard`, `BooleanGuard`, `ArrayGuard`, `ObjectGuard`, `AnyGuard`, `LiteralGuard`, `EnumGuard`, `UnionGuard`, `OptionalGuard`, `NullableGuard`, `NullishGuard`.

## Results

`guard.validate(schema, value)` returns:

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

Use `guard.parse(schema, value)` when you want invalid data to throw.
