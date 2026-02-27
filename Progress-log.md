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
cargo was installed
##### Note : Rustup usage: </p>
   It would either be installed 1st or it would complain that Rust is installed, this behavour can be alterred</p>
   
   ```
    rustup toolchain link system /usr

   ```
   The above extract show that system referres to the currently used version of Rust, one can use another word,</p>
   to describe an installed version, this would be similar to env in Python. </p> 
   One would be given options if trying to install whilst versions exist. </p>
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


#### The std</p>

        Standard library or crate, have many benefits one example can help in the activation of self build modules:</p>

##### Exiting from a programme</p>
```
fn main(){
   std::process::abort();
}
``` 

##### Reading feed :</p>
```
use std::io;
fn main() {
   io::stdn()
   .read_line(&mut guess).expect("The input has encountered an error");
}
```

##### Comparison between two numbers:</p>
```
use std::io;
use rand::Rng;
fn main(){
   let secret_number = rand::thread.rng().gen_range(1..=100);
}
}

}
```





#### Traits examplified:</p>

```

     struct Sheep { naked: bool, name: &'static str }

trait Animal {
    // Associated function signature; `Self` refers to the implementor type.
    fn new(name: &'static str) -> Self;

    // Method signatures; these will return a string.
    fn name(&self) -> &'static str;
    fn noise(&self) -> &'static str;

    // Traits can provide default method definitions.
    fn talk(&self) {
        println!("{} says {}", self.name(), self.noise());
    }
}

impl Sheep {
    fn is_naked(&self) -> bool {
        self.naked
    }

    fn shear(&mut self) {
        if self.is_naked() {
            // Implementor methods can use the implementor's trait methods.
            println!("{} is already naked...", self.name());
        } else {
            println!("{} gets a haircut!", self.name);

            self.naked = true;
        }
    }
}

// Implement the `Animal` trait for `Sheep`.
impl Animal for Sheep {
    // `Self` is the implementor type: `Sheep`.
    fn new(name: &'static str) -> Sheep {
        Sheep { name: name, naked: false }
    }

    fn name(&self) -> &'static str {
        self.name
    }

    fn noise(&self) -> &'static str {
        if self.is_naked() {
            "baaaaah?"
        } else {
            "baaaaah!"
        }
    }

    // Default trait methods can be overridden.
    fn talk(&self) {
        // For example, we can add some quiet contemplation.
        println!("{} pauses briefly... {}", self.name, self.noise());
    }
}

fn main() {
    // Type annotation is necessary in this case.
    let mut dolly: Sheep = Animal::new("Dolly");
    // TODO ^ Try removing the type annotations.

    dolly.talk();
    dolly.shear();
    dolly.talk();
}

```
##### Revision on Rust concepts and constructs: (2026/02/27)</p>

Command line preferences, the usage of cargo check,(saving on the generation of binary file to ensure all</p> components are tested and passing.


#### Mutable variables and the status of variables in general:</p>
The variables are in general immutable, that tends to be the preferred status under Rust. </p> This however tends to be changeable by using mut and associating it with the variable declaration:</p>

```

fn main() {
   let mut statementz = String::from("BlueGreen");
   println!("the statement is {statementz}");
   let mut statementz = String::from("BlueRed");
   println!("The statement is {statementz}");

```



