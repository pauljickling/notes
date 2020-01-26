## Structs

Structs are objects that store particular types of data called *fields*. They are similar to a schema used for a database.

```
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}
```

Once the struct is defined instances of the struct can be created.

```
let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
```

The mutability of a struct's fields is dependent on the instance of the struct.

Dot notation is used to access a field value, for example `user1.email` would return `someone@example.com`.

### Tuple Structs

Structs that use tuple syntax provide an abbreviated struct syntax where the fields go unnamed, and are only specified by type.

```
struct Color(i32, i32, i32);
let black = Color(0, 0, 0);
```

### Methods

Functions specific to structs can be specified using implementation blocks (`impl`).

```
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```
