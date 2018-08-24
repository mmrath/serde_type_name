[![Build Status](https://travis-ci.org/mmrath/serde_type_name.svg?branch=master)](https://travis-ci.org/mmrath/serde_type_name)
[![Current Crates.io Version](https://img.shields.io/crates/v/serde_type_name.svg)](https://crates.io/crates/serde_type_name)

# Serde Type Name
Serde Type Name - lookup name of type for any struct or enum that derives serde Serialize. This is completely based on 

https://stackoverflow.com/questions/46612327/using-serde-to-get-a-types-name-as-string-when-the-type-is-only-serialize

# Usage

```rust
extern crate serde_type_name;
extern crate serde;
#[macro_use]
extern crate serde_derive;

use serde::Serialize;
use serde_type_name::type_name;

#[derive(Serialize)]
enum SimpleEnum {
    A(i32),
    B,
    C { code: i32 },
}

#[derive(Serialize)]
struct SimpleStruct {
    pub a: i32,
}

#[derive(Serialize)]
struct GenericStruct<T: Serialize> {
    pub inner: T,
}

fn main() {
    assert_eq!("SimpleEnum", type_name(&SimpleEnum::A(10)));
    assert_eq!("SimpleEnum", type_name(&SimpleEnum::B));
    assert_eq!("SimpleEnum", type_name(&SimpleEnum::C { code: 32 }));
    
    assert_eq!("SimpleStruct", type_name(&SimpleStruct { a: 20 }));
    
    let gs = GenericStruct { inner: 32 };
    assert_eq!("GenericStruct", type_name(&gs));
    
    let gs_enum = GenericStruct {
        inner: SimpleEnum::B,
    };
    assert_eq!("GenericStruct", type_name(&gs_enum));
    assert_eq!("SimpleEnum", type_name(&gs_enum.inner));
}
```





