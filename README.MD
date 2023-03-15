# rust_syscalls

Single stub direct and indirect syscalling with runtime SSN resolving for Windows.  Forked from [https://github.com/janoglezcampos/rust_syscalls](https://github.com/janoglezcampos/rust_syscalls).



## Features

* Single line for all syscalls
* Function name hashing at compilation time
* Direct or indirect sycalls
* x86_64, WOW64 and x86 native support
* Designed to allow the implementation of custom SSN fetching methods
  


## Usage

1. Add the git repository or local path to the library to your dependencies:

```toml
[dependencies]
rust_syscalls = { path = "C:\\Path\\to\\rust_syscalls" }
```

or

```toml
[dependencies]
rust_syscalls = { git = "https://github.com/rasta-mouse/rust_syscalls" }
```

2. Choose direct or indirect method by setting `_DIRECT_` or `_INDIRECT_` as a feature:

```toml
[dependencies]
rust_syscalls = { path/git = "", features = ["_INDIRECT_"]}
```

or

```toml
[dependencies]
rust_syscalls = { path/git = "", features = ["_DIRECT_"]}
```

3. Import

   
```rust
use rust_syscalls::syscall;
```

4.  Syscall

   
```rust
syscall!("func", arg);
```

---
## Example:

```rust
let mut process_handle: HANDLE = 0;
let oa = unsafe { zeroed::<OBJECT_ATTRIBUTES>() };

let mut cid = unsafe { zeroed::<CLIENT_ID>() };
cid.UniqueProcess = 25772;

let status = unsafe { syscall!(
    "NtOpenProcess",
    &mut process_handle,
    PROCESS_ALL_ACCESS,
    &oa,
    &cid)
};

match status {
    0 => println!("[x] hProcess: 0x{}", process_handle),
    _ => println!("[x] NtOpenProcess failed: {}", status)
};
```
