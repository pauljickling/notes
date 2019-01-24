## Enums

Enums enumerate all possible values. The values in the following example are V4 and V6, and they are referred to as variants of the enum.

```
fn main() {
  enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
  }

  let home = IpAddr::V4(127, 0, 0, 1)
  let loopback = IpAddr::V6(String::from("::1"));
  println!("Home: {:?}", home);
  println!("loopback: {:?}", loopback);

}
```

Enums are commonly paired with structs.

```
enum IpAddrKind {
  V4,
  V6,
}

struct IpAddr {
  kind: IpAddrKind,
  address: String,
}

let home = IpAddr {
  kind: IpAddrKind::V4,
  address: String::from("127.0.0.1"),
};

let loopback = IpAddr {
  kind: IpAddrKind::V6,
  address: String::from("::1"),
}
```

## `Option`

`Option` is a type of enum that indicates that a value can either be something or nothing.
