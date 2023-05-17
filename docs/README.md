# Introduction

Sierra is an intermediate representation for the Starknet language. It
can be used to understand the internals of Cairo contracts as well as
creating entirely new programming languages for Starknet.

**Who this is for**

Learning more about Sierra will be useful for:

- Advanced Cairo developers,
- Cairo security auditors and tool developers,
- Researchers seeking to build knew ZK languages.

**WARNING**

This is a community generated documentation with heavy AI input. Notably:

- It may not be accurate,
- It may not be entirely up-to-date.

**How do I use Sierra when writing smart contracts**

If you are using the [`cairo-template`](https://www.github.com/auditless/cairo-template) to build your
smart contracts, there's a built in command `scarb run sierra` you can use to preview generated Sierra
code for your contracts.

**Feedback**

If you have any feedback, message [@p_e](https://www.twitter.com/p_e) on Twitter or create a PR for https://github.com/sierra-docs/sierra-docs.github.io.

# Libfuncs

Libfuncs are the built-in functions in Sierra and are used to implement all operations.
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

**Arguments:**

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

**Arguments:**

- `RC`: RangeCheck pointer.
- `array`: The array to look up in.
- `index`: The index to look up.

**Returns:**

- `RC'`: Updated RangeCheck pointer.
- `array`: The array.
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

**Arguments:**

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

**Arguments:**

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

**Arguments:**

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

**Arguments:**

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

**Arguments:**

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

**Arguments:**

- `BW` : Bitwise built-in.
- `x`: First `u128`.
- `y`: Second `u128`.

**Returns:**

- Updated Bitwise pointer

**Example:**

```rust
bitwise([1], [2], [3]) -> ([4], [5], [6], [7]);
```

## Boolean Operations

---

### `bool_and_impl`

**Syntax:**

```rust
bool_and_impl(a, b) -> (a & b);
```

**Overview:**

Calculates the logical AND of two bool values.

**Semantics:**

Implemented as a multiplication `a * b`.

**Arguments:**

- `a`: First bool
- `b`: Second bool

**Example:**

```rust
bool_and_impl([1], [2]) -> ([3]);
```

### `bool_not_impl`

**Syntax:**

```rust
bool_not_impl(a) -> (!a);
```

**Overview:**

Calculates the logical negation of a bool value.

**Semantics:**

Implemented as a subtraction `1 - a`.

**Example:**

```rust
bool_not_impl([1]) -> ([2]);
```

### `bool_or_impl`

**Syntax:**

```rust
bool_or_impl(a, b) -> (a | b);
```

**Overview:**

Calculates the logical OR of two bool values.

**Semantics:**

Implemented as `a + b - a * b`.

**Arguments:**

- `a`: First bool
- `b`: Second bool

**Example:**

```rust
bool_or_impl([1], [2]) -> ([3]);
```

### `bool_to_felt252`

**Syntax:**

```rust
bool_to_felt252(a) -> (a);
```

**Overview:**

Casts a boolean value into a felt252 value.

**Semantics:**

No underlying reference changes are required as booleans are implemented as felts.

**Example:**

```rust
bool_to_felt252([1]) -> ([2]);
```

### `bool_xor_impl`

**Syntax:**

```rust
bool_xor_impl(a, b) -> (a ^ b);
```

**Overview:**

Calculates the logical XOR of two bool values.

**Semantics:**

Implemented as `let diff = a - b in diff * diff`.

**Arguments:**

- `a`: First bool
- `b`: Second bool

**Example:**

```rust
bool_xor_impl([1], [2]) -> ([3]);
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

## Starknet Syscalls

---

### `call_contract_syscall`

**Syntax:**

```rust
// Note Sierra statement broken into multiple lines for readability only
// See example below for syntax in use
call_contract_syscall(GB, SB, address, entry_point_selector, calldata) {
  fallthrough(GB1, SB1, return)
  <failure_branch_label>(GB2, SB2, revert_reason)
};
```

**Overview:**

Call a given contract.

**Semantics:**

Invokes the call contract syscall.

**Arguments:**

- `GB`: Gas Built-in
- `SB`: System Built-in
- `address`: The address of the called contract
- `entry_point_selector`: A selector for a function within that contract
- `calldata` : Call arguments

**Returns (Success branch):**

- `GB1`: Updated Gas Built-in
- `SB1`: Updated System Built-in
- `return` : Return data

**Returns (Failure branch):**

- `GB2`: Updated Gas Built-in
- `SB2`: Updated System Built-in
- `revert_reason` : Revert reason

**Example:**

```rust
call_contract_syscall([1], [2], [3], [4], [5]) { fallthrough([6], [7], [8]) 999([9], [10], [11]) };
```

## Misc Operations

---

### `branch_align`

**Syntax:**

```rust
branch_align() -> ();
```

**Overview:**

Equalize environment changes across flow merging paths.

**Semantics:**

Align `ap` (allocation pointer) and gas usage for program locations that can be entered from multiple flow paths.

**Example:**

```rust
branch_align() -> ();
```
