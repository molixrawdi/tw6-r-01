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
