# qex-rs

Rust Query Expressions

A lightweight DSL macro for running SQL-like queries on iterables

```rust
let words = ["apple", "strawberry", "grape", "peach", "banana"];

let word_query = qex!(words -> {
    for word in words
    if word.len() == 5
    return word
});

for word in word_query(words) {
    println!("{}", word);
}

// Prints:
// apple
// grape
// peach
```

This example isn't particularly helpful as we could just do this:

```rust
for word in words {
    if word.len() == 5 {
        println!("{}", word);
    }
}
```

qex shines brightest when things start to get complex.

Here's an example of an inner join:

```rust
struct Order {
    id: u32,
    customer_id: u32,
}

struct Custsomer {
    id: u32,
    name: String,
}

let orders = [/* ... */];
let customers = [/* ... */];

let query = qex!(orders, customers -> {
    for order in orders
    for customer in customers
    if order.id = customer.id
    return (order.id, customer.id, customer.name)
});

for (order_id, customer_id, name) in query(orders, customers) {
    println!("{} {} {}", order_id, customer_id, name);
}
```

yielding:

```rust
let products = [/* ... */];

let query = qex!(products -> {
    for product in products
    yield Avg(product.price)
});

let avg = query(products);
```
