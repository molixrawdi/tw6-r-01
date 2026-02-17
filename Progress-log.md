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
