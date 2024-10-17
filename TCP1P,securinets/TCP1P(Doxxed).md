# Doxxed (unsolved)
`I recently forked a public repository on GitHub. After a few days I deleted my repo. However, my friend informed me that he are still able to access one of my commits from that fork which commit 4bxxxxx. Can you figure out how this happened? See the public repo below.`
pretty new to docker never tried a docker challenge before.

Basically we are given git files to investigate. used [this](https://trufflesecurity.com/blog/anyone-can-access-deleted-and-private-repo-data-github) to learn more  
> learnt about git log and reflog
> after idenitfying the repo and user name we can go to the github page and see only one [commit](https://github.com/notevilcorp/tools/commit/4b155f)
> after this we need to bruteforce the hash to get the deleted commit, where we can find the docker image


``` python
import requests

base_url = "https://github.com/notevilcorp/tools/commit/"

def check_commit(hash_part):
    url = f"{base_url}{hash_part}"
    response = requests.head(url)
    if response.status_code == 200:
        print(f"Valid commit found: {url}")
        return True
    else:
        return False

def find_commit():
    # Check all possible combinations after '4b'
    hex_chars = '0123456789abcdef'
    
    for i in hex_chars:
        for j in hex_chars:
            for k in hex_chars:
                for l in hex_chars:
                    short_hash = f"4b{i}{j}{k}{l}"
                    print(f"Trying hash: {short_hash}")
                    if check_commit(short_hash):
                        return short_hash

found_hash = find_commit()

if found_hash:
    print(f"Suspicious commit found: {found_hash}")
else:
    print("No suspicious commit found starting with '4b'.")
```
> after this we find the docker image which was public and see "exec” being copied to /usr/bin/exec.
> we copy this file to our host and get the file after decmpilation in ghidra.
