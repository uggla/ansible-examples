## Escaping into Bash example

Best solution is to use [Ansi C like string](https://wiki.bash-hackers.org/syntax/quoting#ansi_c_like_strings).

```
docker run --rm -ti -e MYVAR="JJJJ KKK" fedora:30 bash -c $'echo \"${MYVAR}\" | awk \'{ print $NF }\'; echo \"${MYVAR}\"'
KKK
JJJJ KKK
```

## Escaping into Ansible example

The Ansible yaml parser seems to not handle \' correctly so use the hex ascii value \\x27.
```
 __________________
< PLAY [localhost] >
 ------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

 ________________________
< TASK [Gathering Facts] >
 ------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

ok: [localhost]
 _________________
< TASK [shell it] >
 -----------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

changed: [localhost]
 ______________
< TASK [debug] >
 --------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

ok: [localhost] => {
    "nene": {
        "changed": true,
        "cmd": "docker run --rm -ti -e MYVAR=\"JJ-KJJ-RRR TTT\" fedora:30 bash -c $'uname -a ; echo \"${MYVAR}\" | awk -F \\x27-\\x27 \\x27{print $NF}\\x27'",
        "delta": "0:00:00.545145",
        "end": "2019-07-10 00:05:05.105466",
        "failed": false,
        "rc": 0,
        "start": "2019-07-10 00:05:04.560321",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "Linux d3883d4ff32c 5.1.16-300.fc30.x86_64 #1 SMP Wed Jul 3 15:06:51 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux\r\nRRR TTT",
        "stdout_lines": [
            "Linux d3883d4ff32c 5.1.16-300.fc30.x86_64 #1 SMP Wed Jul 3 15:06:51 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux",
            "RRR TTT"
        ]
    }
}
 ____________
< PLAY RECAP >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```
