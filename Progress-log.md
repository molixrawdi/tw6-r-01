2026-01-17</p>
Rustup ssl certificate blocker on ubuntu 24.04 blocker.</p>
Error SSL certificate error</p>
Command : ``` 
           curl --proto '=https' --tlsv1.2 https://sh.rustup.rs | sh </p> 
           ``` 
           </p>
Variant command used to solve issue: </p> 
``` 
         curl --proto '=https' --tlsv1.2 -sSf --insecure https://sh.rustup.rs | sh </p> 
```
</p>
Result same. </p>
Actual solution:</p> 

```
    sudo apt update 
    sudo apt install --reinstall ca-certificates 
    sudo update-ca-certificates 
``` 

2026/02/17| 13:39 </p>
cargo was installed using apt with sudo and Rust version is 1.93.1, </p>
this version is uniform accross all the components </p>
checked, cargo , rustup is 1.28.2. <p>

#### Managing dependencies:</p>
Putting all material dependencies in one place, (Folder) for better referencing:</p>
```
     cargo new get-dependencies
     cd get-dependencies
     cargo add rand@0.8.5 trpl@0.2.0
```

One would get a detailed list of what has been added with details of version.

2026/01/1 | 14:13 Async Programming
Investigate futures : https://crates.io/crates/futures

https://tokio.rs for reliable network applications

#### Safety and speed:<p>
  Are achieved through impplementing at compile time instead of run time,</p> this is partially done by ensuring that abstraction is at a very low or no cost.</p>


#### Using online compiler
   https://play.rust-lang.org/?version=stable&mode=debug&edition=2015

#### Memory safety: </p>

   How Rust Prevents Double Free
   Ownership Rules: When a variable goes out of scope, its destructor runs, freeing the heap memory. If the variable      was moved (e.g., let b = a;), the original variable a cannot be used, so its destructor does not run twice.
   No Implicit Copy: Types containing allocated memory (like Box<T>, Vec<T>) do not implement Copy, preventing       
   accidental cloning of pointers that would lead to double free. 
   Double Free in Unsafe Rust 
   Unsafe Code: Manually creating pointers or using std::ptr::read() without properly managing the underlying memory      (e.g., with std::mem::forget()) can cause a Drop to occur on already freed memory, resulting in a runtime crash.
   FFI/External Code: When interfacing with C, if Rust’s Drop is called on a pointer that the C code has already          free()'d, a double-free will occur. 
   Detection and Prevention
  
  ##### Miri: 
  Use the cargo miri tool to detect unsafe behavior and potential memory errors like double-frees during testing.
  Proper Ownership: Avoid using unsafe blocks for memory management whenever possible, relying on Vec, Box, and smart    pointers. 
