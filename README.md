# TypeID Go

### A golang implementation of [TypeIDs](https://github.com/jetify-com/typeid)

![License: Apache 2.0](https://img.shields.io/github/license/jetify-com/typeid-go) [![Go Reference](https://pkg.go.dev/badge/go.jetify.com/typeid.svg)](https://pkg.go.dev/go.jetify.com/typeid)

TypeIDs are a modern, **type-safe**, globally unique identifier based on the upcoming
UUIDv7 standard. They provide a ton of nice properties that make them a great choice
as the primary identifiers for your data in a database, APIs, and distributed systems.
Read more about TypeIDs in their [spec](https://github.com/jetify-com/typeid).

This particular implementation provides a go library for generating and parsing TypeIDs.

## Installation

To add this library as a dependency in your go module, run:

```bash
go get go.jetify.com/typeid/v2
```

## Usage

This library provides a go implementation of TypeID:

```go
import (
  "go.jetify.com/typeid/v2"
)

func example() {
  // Generate a new TypeID with a prefix (panics on invalid prefix)
  tid := typeid.MustGenerate("user")
  fmt.Println(tid)
  
  // Generate a new TypeID without a prefix
  tid = typeid.MustGenerate("")
  fmt.Println(tid)
  
  // Generate with error handling
  tid, err := typeid.Generate("user")
  if err != nil {
    log.Fatal(err)
  }
  
  // Parse an existing TypeID
  tid, _ = typeid.Parse("user_00041061050r3gg28a1c60t3gf")
  fmt.Println(tid)
  
  // Convert from UUID
  tid, _ = typeid.FromUUID("user", "018e5f71-6f04-7c5c-8123-456789abcdef")
  fmt.Println(tid)
}

`SqlAlchemy Support`
The library also allows to create classes specifically for the use with sqlalchemy.

`Example`
  from typeid import generate

typeids = generate("user")

```bash
class User(Base):
    __tablename__ = "user"

    # use typeids.UserId in the Mapper[]
    id: Mapped[typeids.UserId] = mapped_column(
        # use typeids.UserId in the mapped_column!
        typeids.UserId, primary_key=True, default=uuid.uuid4
    )

    name: str = Column(String(128), unique=True)

user = User(name="John Doe")
(db,) = get_session()
db.add(user)
db.commit()
db.refresh(user)
print(type(user.id))
# <class 'typeid.UserId'>

print((user.id))
# user_qvnrsem3skqho2hgni2mfk```

For the full documentation, see this package's [godoc](https://pkg.go.dev/go.jetify.com/typeid).
