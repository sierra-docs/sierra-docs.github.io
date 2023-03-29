# Community documentation for Sierra

Sierra is an intermediate representation for the Starknet language. It
can be used to understand the internals of Cairo contracts as well as
creating entirely new programming languages for Starknet.

**WARNING**

This is a community generated documentation with heavy AI input. Notably:

- It may not be accurate,
- It may not be entirely up-to-date.

**Feedback**

If you have any feedback, message [@p_e](http://www.twitter.com/p_e) on Twitter or create a PR for https://github.com/sierra-docs/sierra-docs.github.io.

# Libfuncs

Libfuncs are the built-in functions in Sierra. They are used to implement all operations.
Libfuncs are polymorphic supporting multiple types.

## Array Operations

---

### `array_append`

**Syntax:**

```rust
array_append<T>(array, element) -> (new_array);
```

**Overview:**

Appends an element to the end of an array.

**Semantics:**

The `array_append` function takes an array and an element as input. It handles updating the array's start and end pointer.

**Parameters:**

- `array`: The array to which the element should be appended.
- `element`: The element of type `T` that will be appended to the array.

**Example:**

```rust
array_append<felt252>([1], [2]) -> ([3]);
```

### `array_get`

**Syntax:**

```rust
array_get<T>(RC, array, index) -> (RC', array, element);
```

**Overview:**

Fetch an element at a specific index.

**Semantics:**

Generates the range checks required to check index is within bounds and returns the element at a given index.

**Parameters:**

- `RC`: RangeCheck pointer.
- `array`: The array to look up in.
- `index`: The index to look up.

**Returns:**

- `RC'`: Updated RangeCheck pointer.
- `array`: The array
- `element` : The element at the given index.

**Example:**

```rust
array_get<felt252>([1], [2], [3]) -> ([4], [5], [6]);
```

### `array_len`

**Syntax:**

```rust
array_len<T>(array) -> (length);
```

**Overview:**

Get the length of an array.

**Parameters:**

- `array`: The array to measure.

**Example:**

```rust
array_len<felt252>([1]) -> ([2]);
```

### `array_new`

**Syntax:**

```rust
array_new<T>() -> (array);
```

**Overview:**

Allocate space for a new array and return the start pointer.

**Semantics:**

Allocates the memory segment for a new array.

**Example:**

```rust
array_new<felt252>() -> ([1]);
```

### `array_pop_front`

**Syntax:**

```rust
array_pop_front<T>(array) -> (array, element);
```

**Overview:**

Remove an element from the beginning of an array.

**Semantics:**

Remove and return the first element of an array.

**Parameters:**

- `array`: The array to manipulate.

**Example:**

```rust
array_pop_front<felt252>([1]) -> ([2], [3]);
```

### `array_slice`

**Syntax:**

```rust
array_slice<T>(array, start, length) -> (slice);
```

**Overview:**

Return a slice of an array.

**Semantics:**

Returns a snapshot pointer to an array slice starting at `start` of length `length`.

**Parameters:**

- `array`: The array to slice.
- `start`: Start of a slice.
- `length`: Length of the slice.

**Example:**

```rust
array_slice<felt252>([1], [2], [3]) -> ([4]);
```

### `array_snapshot_pop_back`

**Syntax:**

```rust
array_snapshot_pop_back<T>(@array) -> (@array, @element);
```

**Overview:**

Remove the last element of the array and return a snapshot of it.

**Semantics:**

Returns a snapshot pointer to the last element of the array and removes it.

**Parameters:**

- `array`: The snapshot of the array to manipulate.

**Example:**

```rust
array_snapshot_pop_back<felt252>([1]) -> ([2], [3]);
```

### `array_snapshot_pop_front`

**Syntax:**

```rust
array_snapshot_pop_front<T>(@array) -> (@array, @element);
```

**Overview:**

Remove the first element of the array and return a snapshot of it.

**Semantics:**

Returns a snapshot pointer to the first element of the array and removes it.

**Parameters:**

- `array`: The snapshot of the array to manipulate.

**Example:**

```rust
array_snapshot_pop_front<felt252>([1]) -> ([2], [3]);
```

## Bitwise Operations

---

### `bitwise`

**Syntax:**

```rust
bitwise(BW, x, y) -> (x & y, x ^ y, x | y, BW');
```

**Overview:**

Computes all bitwise operations (AND, XOR, OR) for two `u128` types.

**Semantics:**

Invokes the bitwise built-in.

**Parameters:**

- `BW` : Bitwise built-in.
- `x`: First boolean.
- `y`: Second boolean.

**Returns:**

- Updated Bitwise pointer

**Example:**

```rust
bitwise([1], [2], [3]) -> ([4], [5], [6], [7]);
```

## Memory Operations

---

### `alloc_local`

**Syntax:**

```rust
alloc_local<T>() -> (variable);
```

**Overview:**

Allocates space for new local variable.

**Semantics:**

Allocates space in the frame. Handles offset calculations based on the size of the type allocated.

**Example:**

```rust
alloc_local<felt252>() -> ([1]);
```

### `finalize_locals`

**Syntax:**

```rust
finalize_locals() -> ();
```

**Overview:**

Finalizes the allocation of local variables.

**Semantics:**

Finalizes the allocation of local variables and calculates the new frame state.

### `rename`

**Syntax:**

```rust
rename<T>(old) -> (new);
```

**Overview:**

Rename identifier.

**Arguments:**

Identifier to rename.

**Semantics:**

Used to align identities in a flow control merge.

**Example:**

```rust
rename<felt252>([1]) -> ([2]);
```

### `store_local`

**Syntax:**

```rust
store_local<T>(destination, source) -> (newvalue);
```

**Overview:**

Stores source variable into destination variable locally.

**Arguments:**

A reference to the destination (local variable) and source value.

**Semantics:**

Returns updated value of local variable after storage operation.

**Example:**

```rust
store_local<felt252>([1], [2]) -> ([2]);
```

### `store_temp`

**Syntax:**

```rust
store_temp<T>(value) -> (new_value);
```

**Overview:**

Store value into temporary memory.

**Arguments:**

Value to be stored.

**Example:**

```rust
store_temp<felt252>([1]) -> ([1]);
```