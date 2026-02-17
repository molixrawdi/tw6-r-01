2026-01-17
Rustup ssl certificate blocker on ubuntu 24.04 blocker.
Error SSL certificate error
Command : ''' 
           curl --proto '=https' --tlsv1.2 https://sh.rustup.rs | sh 
           '''
Variant command used to solve issue: 
''' 
         curl --proto '=https' --tlsv1.2 -sSf --insecure https://sh.rustup.rs | sh 
'''
Result same.
Actual solution: 
'''
    sudo apt update
    sudo apt install --reinstall ca-certificates
    sudo update-ca-certificates
'''

2026/02/17| 13:39
cargo was installed using apt with sudo and Rust version is 1.93.1, this version is uniform accross all the components
checked, cargo , rustup is 1.28.2.
