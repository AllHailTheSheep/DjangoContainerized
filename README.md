Dockerfile.naive contains an all-in-one image, not sutiable for production.
Dockerfile.multistage contains a base, build, and a distroless runtime image.

This slims our output down from 392MB for the naive build to 139MB for the multistage build, a ~65% reduction in attack surface. This is reflected by scanning the images with `trivy`.

For our naive build:
```
localhost/django-naive:latest (debian 12.12)

Total: 84 (UNKNOWN: 0, LOW: 64, MEDIUM: 14, HIGH: 4, CRITICAL: 2)
```
And for our multistage, distroless build:
```
localhost/django-multistage-distroless:latest (debian 12.12)

Total: 53 (UNKNOWN: 0, LOW: 32, MEDIUM: 16, HIGH: 3, CRITICAL: 2)
```

Full `trivy` output for the naive build:
```
julia@juliadesktop:~/.local/bin$ ./trivy image --quiet localhost/django-naive:latest

Report Summary

┌──────────────────────────────────────────────────────────────────────────────────┬────────────┬─────────────────┬─────────┐
│                                      Target                                      │    Type    │ Vulnerabilities │ Secrets │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ localhost/django-naive:latest (debian 12.12)                                     │   debian   │       84        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/asgiref-3.10.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/django-5.2.8.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/pip-25.1.1.dist-info/METADATA             │ python-pkg │        1        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/sqlparse-0.5.3.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/asgiref-3.10.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/asttokens-3.0.1.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/black-25.11.0.dist-info/METADATA          │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/click-8.3.1.dist-info/METADATA            │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/decorator-5.2.1.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/django-5.2.8.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/django_debug_toolbar-6.1.0.dist-info/MET- │ python-pkg │        0        │    -    │
│ ADATA                                                                            │            │                 │         │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/django_environ-0.12.0.dist-info/METADATA  │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/executing-2.2.1.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/flake8-7.3.0.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/gunicorn-23.0.0.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/iniconfig-2.3.0.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/ipython-9.7.0.dist-info/METADATA          │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/ipython_pygments_lexers-1.1.1.dist-info/- │ python-pkg │        0        │    -    │
│ METADATA                                                                         │            │                 │         │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/isort-7.0.0.dist-info/METADATA            │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/jedi-0.19.2.dist-info/METADATA            │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/matplotlib_inline-0.2.1.dist-info/METADA- │ python-pkg │        0        │    -    │
│ TA                                                                               │            │                 │         │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/mccabe-0.7.0.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/mypy-1.18.2.dist-info/METADATA            │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/mypy_extensions-1.1.0.dist-info/METADATA  │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/packaging-25.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/parso-0.8.5.dist-info/METADATA            │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pathspec-0.12.1.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pexpect-4.9.0.dist-info/METADATA          │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pillow-12.0.0.dist-info/METADATA          │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pip-25.2.dist-info/METADATA               │ python-pkg │        1        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/platformdirs-4.5.0.dist-info/METADATA     │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pluggy-1.6.0.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/prompt_toolkit-3.0.52.dist-info/METADATA  │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/psycopg-3.2.12.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/psycopg_binary-3.2.12.dist-info/METADATA  │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/psycopg_pool-3.2.7.dist-info/METADATA     │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/ptyprocess-0.7.0.dist-info/METADATA       │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pure_eval-0.2.3.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pycodestyle-2.14.0.dist-info/METADATA     │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pyflakes-3.4.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pygments-2.19.2.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pytest-9.0.1.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pytest_django-4.11.1.dist-info/METADATA   │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/pytokens-0.3.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/sqlparse-0.5.3.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/stack_data-0.6.3.dist-info/METADATA       │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/traitlets-5.14.3.dist-info/METADATA       │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/typing_extensions-4.15.0.dist-info/METAD- │ python-pkg │        0        │    -    │
│ ATA                                                                              │            │                 │         │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/watchdog-6.0.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/wcwidth-0.2.14.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ usr/local/lib/python3.13/site-packages/whitenoise-6.11.0.dist-info/METADATA      │ python-pkg │        0        │    -    │
└──────────────────────────────────────────────────────────────────────────────────┴────────────┴─────────────────┴─────────┘
Legend:
- '-': Not scanned
- '0': Clean (no security findings detected)


localhost/django-naive:latest (debian 12.12)

Total: 84 (UNKNOWN: 0, LOW: 64, MEDIUM: 14, HIGH: 4, CRITICAL: 2)

┌────────────────────┬─────────────────────┬──────────┬──────────────┬────────────────────────┬───────────────┬──────────────────────────────────────────────────────────────┐
│      Library       │    Vulnerability    │ Severity │    Status    │   Installed Version    │ Fixed Version │                            Title                             │
├────────────────────┼─────────────────────┼──────────┼──────────────┼────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ apt                │ CVE-2011-3374       │ LOW      │ affected     │ 2.6.1                  │               │ It was found that apt-key in apt, all versions, do not       │
│                    │                     │          │              │                        │               │ correctly...                                                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2011-3374                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ bash               │ TEMP-0841856-B18BAF │          │              │ 5.2.15-2+b9            │               │ [Privilege escalation possible to other user than root]      │
│                    │                     │          │              │                        │               │ https://security-tracker.debian.org/tracker/TEMP-0841856-B1- │
│                    │                     │          │              │                        │               │ 8BAF                                                         │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ bsdutils           │ CVE-2022-0563       │          │              │ 1:2.38.1-5+deb12u3     │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┤          ├──────────────┼────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ coreutils          │ CVE-2016-2781       │          │ will_not_fix │ 9.1-1                  │               │ coreutils: Non-privileged session can escape to the parent   │
│                    │                     │          │              │                        │               │ session in chroot                                            │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2016-2781                    │
│                    ├─────────────────────┤          ├──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2017-18018      │          │ affected     │                        │               │ coreutils: race condition vulnerability in chown and chgrp   │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2017-18018                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-5278       │          │              │                        │               │ coreutils: Heap Buffer Under-Read in GNU Coreutils sort via  │
│                    │                     │          │              │                        │               │ Key Specification                                            │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-5278                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ dpkg               │ CVE-2025-6297       │          │              │ 1.21.22                │               │ It was discovered that dpkg-deb does not properly sanitize   │
│                    │                     │          │              │                        │               │ directory p ......                                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6297                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ gcc-12-base        │ CVE-2022-27943      │          │              │ 12.2.0-14+deb12u1      │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows   │
│                    │                     │          │              │                        │               │ stack exhaustion in demangle_const                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-27943                   │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ gpgv               │ CVE-2025-30258      │ MEDIUM   │              │ 2.2.40-1.1+deb12u1     │               │ gnupg: verification DoS due to a malicious subkey in the     │
│                    │                     │          │              │                        │               │ keyring                                                      │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-30258                   │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2022-3219       │ LOW      │              │                        │               │ gnupg: denial of service issue (resource consumption) using  │
│                    │                     │          │              │                        │               │ compressed packets                                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-3219                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libapt-pkg6.0      │ CVE-2011-3374       │          │              │ 2.6.1                  │               │ It was found that apt-key in apt, all versions, do not       │
│                    │                     │          │              │                        │               │ correctly...                                                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2011-3374                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libblkid1          │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u3       │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libc-bin           │ CVE-2010-4756       │          │              │ 2.36-9+deb12u13        │               │ glibc: glob implementation can cause excessive CPU and       │
│                    │                     │          │              │                        │               │ memory consumption due to...                                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2010-4756                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-20796      │          │              │                        │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                        │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2018-20796                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010022    │          │              │                        │               │ glibc: stack guard protection bypass                         │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010022                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010023    │          │              │                        │               │ glibc: running ldd on malicious ELF leads to code execution  │
│                    │                     │          │              │                        │               │ because of...                                                │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010023                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010024    │          │              │                        │               │ glibc: ASLR bypass using cache of thread stack and heap      │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010024                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010025    │          │              │                        │               │ glibc: information disclosure of heap addresses of           │
│                    │                     │          │              │                        │               │ pthread_created thread                                       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010025                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-9192       │          │              │                        │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                        │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-9192                    │
├────────────────────┼─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libc6              │ CVE-2010-4756       │          │              │                        │               │ glibc: glob implementation can cause excessive CPU and       │
│                    │                     │          │              │                        │               │ memory consumption due to...                                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2010-4756                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2018-20796      │          │              │                        │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                        │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2018-20796                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010022    │          │              │                        │               │ glibc: stack guard protection bypass                         │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010022                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010023    │          │              │                        │               │ glibc: running ldd on malicious ELF leads to code execution  │
│                    │                     │          │              │                        │               │ because of...                                                │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010023                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010024    │          │              │                        │               │ glibc: ASLR bypass using cache of thread stack and heap      │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010024                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-1010025    │          │              │                        │               │ glibc: information disclosure of heap addresses of           │
│                    │                     │          │              │                        │               │ pthread_created thread                                       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-1010025                 │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2019-9192       │          │              │                        │               │ glibc: uncontrolled recursion in function                    │
│                    │                     │          │              │                        │               │ check_dst_limits_calc_pos_1 in posix/regexec.c               │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2019-9192                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgcc-s1          │ CVE-2022-27943      │          │              │ 12.2.0-14+deb12u1      │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows   │
│                    │                     │          │              │                        │               │ stack exhaustion in demangle_const                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-27943                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgcrypt20        │ CVE-2018-6829       │          │              │ 1.10.1-3               │               │ libgcrypt: ElGamal implementation doesn't have semantic      │
│                    │                     │          │              │                        │               │ security due to incorrectly encoded plaintexts...            │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2018-6829                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-2236       │          │              │                        │               │ libgcrypt: vulnerable to Marvin Attack                       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-2236                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libgnutls30        │ CVE-2011-3389       │          │              │ 3.7.9-2+deb12u5        │               │ HTTPS: block-wise chosen-plaintext attack against SSL/TLS    │
│                    │                     │          │              │                        │               │ (BEAST)                                                      │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2011-3389                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libmount1          │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u3       │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libncursesw6       │ CVE-2023-50495      │ MEDIUM   │              │ 6.4-4                  │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-6141       │ LOW      │              │                        │               │ gnu-ncurses: ncurses Stack Buffer Overflow                   │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6141                    │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libpam-modules     │ CVE-2025-6020       │ HIGH     │              │ 1.5.2-6+deb12u1        │               │ linux-pam: Linux-pam directory Traversal                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6020                    │
│                    ├─────────────────────┼──────────┼──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-10041      │ MEDIUM   │ will_not_fix │                        │               │ pam: libpam: Libpam vulnerable to read hashed password       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-10041                   │
│                    ├─────────────────────┤          ├──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-22365      │          │ affected     │                        │               │ pam: allowing unprivileged user to block another user        │
│                    │                     │          │              │                        │               │ namespace                                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-22365                   │
├────────────────────┼─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libpam-modules-bin │ CVE-2025-6020       │ HIGH     │              │                        │               │ linux-pam: Linux-pam directory Traversal                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6020                    │
│                    ├─────────────────────┼──────────┼──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-10041      │ MEDIUM   │ will_not_fix │                        │               │ pam: libpam: Libpam vulnerable to read hashed password       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-10041                   │
│                    ├─────────────────────┤          ├──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-22365      │          │ affected     │                        │               │ pam: allowing unprivileged user to block another user        │
│                    │                     │          │              │                        │               │ namespace                                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-22365                   │
├────────────────────┼─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libpam-runtime     │ CVE-2025-6020       │ HIGH     │              │                        │               │ linux-pam: Linux-pam directory Traversal                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6020                    │
│                    ├─────────────────────┼──────────┼──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-10041      │ MEDIUM   │ will_not_fix │                        │               │ pam: libpam: Libpam vulnerable to read hashed password       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-10041                   │
│                    ├─────────────────────┤          ├──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-22365      │          │ affected     │                        │               │ pam: allowing unprivileged user to block another user        │
│                    │                     │          │              │                        │               │ namespace                                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-22365                   │
├────────────────────┼─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│ libpam0g           │ CVE-2025-6020       │ HIGH     │              │                        │               │ linux-pam: Linux-pam directory Traversal                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6020                    │
│                    ├─────────────────────┼──────────┼──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-10041      │ MEDIUM   │ will_not_fix │                        │               │ pam: libpam: Libpam vulnerable to read hashed password       │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-10041                   │
│                    ├─────────────────────┤          ├──────────────┤                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-22365      │          │ affected     │                        │               │ pam: allowing unprivileged user to block another user        │
│                    │                     │          │              │                        │               │ namespace                                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-22365                   │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libsmartcols1      │ CVE-2022-0563       │ LOW      │              │ 2.38.1-5+deb12u3       │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libsqlite3-0       │ CVE-2025-7458       │ CRITICAL │              │ 3.40.1-2+deb12u2       │               │ sqlite: SQLite integer overflow                              │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-7458                    │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-7709       │ MEDIUM   │              │                        │               │ An integer overflow exists in the FTS5                       │
│                    │                     │          │              │                        │               │ https://sqlite.org/fts5.html e ...                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-7709                    │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2021-45346      │ LOW      │              │                        │               │ sqlite: crafted SQL query allows a malicious user to obtain  │
│                    │                     │          │              │                        │               │ sensitive information...                                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2021-45346                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-29088      │          │              │                        │               │ sqlite: Denial of Service in SQLite                          │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-29088                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-52099      │          │              │                        │               │ sqlite: SQLite integer overflow                              │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-52099                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libssl3            │ CVE-2025-27587      │          │              │ 3.0.17-1~deb12u3       │               │ OpenSSL 3.0.0 through 3.3.2 on the PowerPC architecture is   │
│                    │                     │          │              │                        │               │ vulnerable ......                                            │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-27587                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libstdc++6         │ CVE-2022-27943      │          │              │ 12.2.0-14+deb12u1      │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows   │
│                    │                     │          │              │                        │               │ stack exhaustion in demangle_const                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-27943                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libsystemd0        │ CVE-2013-4392       │          │              │ 252.39-1~deb12u1       │               │ systemd: TOCTOU race condition when updating file            │
│                    │                     │          │              │                        │               │ permissions and SELinux security contexts...                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2013-4392                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31437      │          │              │                        │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                        │               │ modify a...                                                  │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31437                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31438      │          │              │                        │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                        │               │ truncate a...                                                │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31438                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31439      │          │              │                        │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                        │               │ modify the...                                                │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31439                   │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libtinfo6          │ CVE-2023-50495      │ MEDIUM   │              │ 6.4-4                  │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-6141       │ LOW      │              │                        │               │ gnu-ncurses: ncurses Stack Buffer Overflow                   │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6141                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libudev1           │ CVE-2013-4392       │          │              │ 252.39-1~deb12u1       │               │ systemd: TOCTOU race condition when updating file            │
│                    │                     │          │              │                        │               │ permissions and SELinux security contexts...                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2013-4392                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31437      │          │              │                        │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                        │               │ modify a...                                                  │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31437                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31438      │          │              │                        │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                        │               │ truncate a...                                                │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31438                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31439      │          │              │                        │               │ An issue was discovered in systemd 253. An attacker can      │
│                    │                     │          │              │                        │               │ modify the...                                                │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31439                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libuuid1           │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u3       │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ login              │ CVE-2007-5686       │          │              │ 1:4.13+dfsg1-1+deb12u1 │               │ initscripts in rPath Linux 1 sets insecure permissions for   │
│                    │                     │          │              │                        │               │ the /var/lo ......                                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2007-5686                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-56433      │          │              │                        │               │ shadow-utils: Default subordinate ID configuration in        │
│                    │                     │          │              │                        │               │ /etc/login.defs could lead to compromise                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-56433                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ TEMP-0628843-DBAD28 │          │              │                        │               │ [more related to CVE-2005-4890]                              │
│                    │                     │          │              │                        │               │ https://security-tracker.debian.org/tracker/TEMP-0628843-DB- │
│                    │                     │          │              │                        │               │ AD28                                                         │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ mount              │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u3       │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┼─────────────────────┼──────────┤              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ ncurses-base       │ CVE-2023-50495      │ MEDIUM   │              │ 6.4-4                  │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-6141       │ LOW      │              │                        │               │ gnu-ncurses: ncurses Stack Buffer Overflow                   │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6141                    │
├────────────────────┼─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│ ncurses-bin        │ CVE-2023-50495      │ MEDIUM   │              │                        │               │ ncurses: segmentation fault via _nc_wrap_entry()             │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-50495                   │
│                    ├─────────────────────┼──────────┤              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2025-6141       │ LOW      │              │                        │               │ gnu-ncurses: ncurses Stack Buffer Overflow                   │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-6141                    │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ openssl            │ CVE-2025-27587      │          │              │ 3.0.17-1~deb12u3       │               │ OpenSSL 3.0.0 through 3.3.2 on the PowerPC architecture is   │
│                    │                     │          │              │                        │               │ vulnerable ......                                            │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2025-27587                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ passwd             │ CVE-2007-5686       │          │              │ 1:4.13+dfsg1-1+deb12u1 │               │ initscripts in rPath Linux 1 sets insecure permissions for   │
│                    │                     │          │              │                        │               │ the /var/lo ......                                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2007-5686                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2024-56433      │          │              │                        │               │ shadow-utils: Default subordinate ID configuration in        │
│                    │                     │          │              │                        │               │ /etc/login.defs could lead to compromise                     │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2024-56433                   │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ TEMP-0628843-DBAD28 │          │              │                        │               │ [more related to CVE-2005-4890]                              │
│                    │                     │          │              │                        │               │ https://security-tracker.debian.org/tracker/TEMP-0628843-DB- │
│                    │                     │          │              │                        │               │ AD28                                                         │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ perl-base          │ CVE-2011-4116       │          │              │ 5.36.0-7+deb12u3       │               │ perl: File:: Temp insecure temporary file handling           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2011-4116                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ CVE-2023-31486      │          │              │                        │               │ http-tiny: insecure TLS cert default                         │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-31486                   │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ sysvinit-utils     │ TEMP-0517018-A83CE6 │          │              │ 3.06-4                 │               │ [sysvinit: no-root option in expert installer exposes        │
│                    │                     │          │              │                        │               │ locally exploitable security flaw]                           │
│                    │                     │          │              │                        │               │ https://security-tracker.debian.org/tracker/TEMP-0517018-A8- │
│                    │                     │          │              │                        │               │ 3CE6                                                         │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ tar                │ CVE-2005-2541       │          │              │ 1.34+dfsg-1.2+deb12u1  │               │ tar: does not properly warn the user when extracting setuid  │
│                    │                     │          │              │                        │               │ or setgid...                                                 │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2005-2541                    │
│                    ├─────────────────────┤          │              │                        ├───────────────┼──────────────────────────────────────────────────────────────┤
│                    │ TEMP-0290435-0B57B5 │          │              │                        │               │ [tar's rmt command may have undesired side effects]          │
│                    │                     │          │              │                        │               │ https://security-tracker.debian.org/tracker/TEMP-0290435-0B- │
│                    │                     │          │              │                        │               │ 57B5                                                         │
├────────────────────┼─────────────────────┤          │              ├────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ util-linux         │ CVE-2022-0563       │          │              │ 2.38.1-5+deb12u3       │               │ util-linux: partial disclosure of arbitrary files in chfn    │
│                    │                     │          │              │                        │               │ and chsh when compiled...                                    │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
├────────────────────┤                     │          │              │                        ├───────────────┤                                                              │
│ util-linux-extra   │                     │          │              │                        │               │                                                              │
│                    │                     │          │              │                        │               │                                                              │
│                    │                     │          │              │                        │               │                                                              │
├────────────────────┼─────────────────────┼──────────┼──────────────┼────────────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ zlib1g             │ CVE-2023-45853      │ CRITICAL │ will_not_fix │ 1:1.2.13.dfsg-1        │               │ zlib: integer overflow and resultant heap-based buffer       │
│                    │                     │          │              │                        │               │ overflow in zipOpenNewFileInZip4_6                           │
│                    │                     │          │              │                        │               │ https://avd.aquasec.com/nvd/cve-2023-45853                   │
└────────────────────┴─────────────────────┴──────────┴──────────────┴────────────────────────┴───────────────┴──────────────────────────────────────────────────────────────┘

Python (python-pkg)

Total: 2 (UNKNOWN: 0, LOW: 0, MEDIUM: 2, HIGH: 0, CRITICAL: 0)

┌────────────────┬───────────────┬──────────┬────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────┐
│    Library     │ Vulnerability │ Severity │ Status │ Installed Version │ Fixed Version │                        Title                        │
├────────────────┼───────────────┼──────────┼────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────┤
│ pip (METADATA) │ CVE-2025-8869 │ MEDIUM   │ fixed  │ 25.1.1            │ 25.3          │ pip: pip missing checks on symbolic link extraction │
│                │               │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8869           │
│                │               │          │        ├───────────────────┤               │                                                     │
│                │               │          │        │ 25.2              │               │                                                     │
│                │               │          │        │                   │               │                                                     │
└────────────────┴───────────────┴──────────┴────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────┘
```
And full `trivy` output for the multistage build:
```
julia@juliadesktop:~/.local/bin$ ./trivy image --quiet localhost/django-multistage-distroless:latest

Report Summary

┌──────────────────────────────────────────────────────────────────────────────────┬────────────┬─────────────────┬─────────┐
│                                      Target                                      │    Type    │ Vulnerabilities │ Secrets │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ localhost/django-multistage-distroless:latest (debian 12.12)                     │   debian   │       53        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/asgiref-3.10.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/django-5.2.8.dist-info/METADATA           │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/django_environ-0.12.0.dist-info/METADATA  │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/gunicorn-23.0.0.dist-info/METADATA        │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/packaging-25.0.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/pillow-12.0.0.dist-info/METADATA          │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/pip-25.2.dist-info/METADATA               │ python-pkg │        1        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/psycopg-3.2.12.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/psycopg_binary-3.2.12.dist-info/METADATA  │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/psycopg_pool-3.2.7.dist-info/METADATA     │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/sqlparse-0.5.3.dist-info/METADATA         │ python-pkg │        0        │    -    │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/typing_extensions-4.15.0.dist-info/METAD- │ python-pkg │        0        │    -    │
│ ATA                                                                              │            │                 │         │
├──────────────────────────────────────────────────────────────────────────────────┼────────────┼─────────────────┼─────────┤
│ app/.venv/lib/python3.13/site-packages/whitenoise-6.11.0.dist-info/METADATA      │ python-pkg │        0        │    -    │
└──────────────────────────────────────────────────────────────────────────────────┴────────────┴─────────────────┴─────────┘
Legend:
- '-': Not scanned
- '0': Clean (no security findings detected)


localhost/django-multistage-distroless:latest (debian 12.12)

Total: 53 (UNKNOWN: 0, LOW: 32, MEDIUM: 16, HIGH: 3, CRITICAL: 2)

┌───────────────────────┬──────────────────┬──────────┬──────────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
│        Library        │  Vulnerability   │ Severity │    Status    │ Installed Version │ Fixed Version │                            Title                            │
├───────────────────────┼──────────────────┼──────────┼──────────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ gcc-12-base           │ CVE-2022-27943   │ LOW      │ affected     │ 12.2.0-14+deb12u1 │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows  │
│                       │                  │          │              │                   │               │ stack exhaustion in demangle_const                          │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2022-27943                  │
├───────────────────────┼──────────────────┤          │              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libc6                 │ CVE-2010-4756    │          │              │ 2.36-9+deb12u13   │               │ glibc: glob implementation can cause excessive CPU and      │
│                       │                  │          │              │                   │               │ memory consumption due to...                                │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2010-4756                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2018-20796   │          │              │                   │               │ glibc: uncontrolled recursion in function                   │
│                       │                  │          │              │                   │               │ check_dst_limits_calc_pos_1 in posix/regexec.c              │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2018-20796                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2019-1010022 │          │              │                   │               │ glibc: stack guard protection bypass                        │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2019-1010022                │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2019-1010023 │          │              │                   │               │ glibc: running ldd on malicious ELF leads to code execution │
│                       │                  │          │              │                   │               │ because of...                                               │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2019-1010023                │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2019-1010024 │          │              │                   │               │ glibc: ASLR bypass using cache of thread stack and heap     │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2019-1010024                │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2019-1010025 │          │              │                   │               │ glibc: information disclosure of heap addresses of          │
│                       │                  │          │              │                   │               │ pthread_created thread                                      │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2019-1010025                │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2019-9192    │          │              │                   │               │ glibc: uncontrolled recursion in function                   │
│                       │                  │          │              │                   │               │ check_dst_limits_calc_pos_1 in posix/regexec.c              │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2019-9192                   │
├───────────────────────┼──────────────────┼──────────┼──────────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libexpat1             │ CVE-2025-59375   │ MEDIUM   │ will_not_fix │ 2.5.0-1+deb12u2   │               │ expat: libexpat in Expat allows attackers to trigger large  │
│                       │                  │          │              │                   │               │ dynamic memory allocations...                               │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-59375                  │
│                       ├──────────────────┼──────────┼──────────────┤                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2023-52426   │ LOW      │ affected     │                   │               │ expat: recursive XML entity expansion vulnerability         │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2023-52426                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-28757   │          │              │                   │               │ expat: XML Entity Expansion                                 │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-28757                  │
├───────────────────────┼──────────────────┤          │              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libgcc-s1             │ CVE-2022-27943   │          │              │ 12.2.0-14+deb12u1 │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows  │
│                       │                  │          │              │                   │               │ stack exhaustion in demangle_const                          │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2022-27943                  │
├───────────────────────┤                  │          │              │                   ├───────────────┤                                                             │
│ libgomp1              │                  │          │              │                   │               │                                                             │
│                       │                  │          │              │                   │               │                                                             │
│                       │                  │          │              │                   │               │                                                             │
├───────────────────────┼──────────────────┤          │              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libgssapi-krb5-2      │ CVE-2018-5709    │          │              │ 1.20.1-2+deb12u4  │               │ krb5: integer overflow in dbentry->n_key_data in            │
│                       │                  │          │              │                   │               │ kadmin/dbutil/dump.c                                        │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2018-5709                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26458   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c           │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26458                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26461   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c   │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26461                  │
├───────────────────────┼──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│ libk5crypto3          │ CVE-2018-5709    │          │              │                   │               │ krb5: integer overflow in dbentry->n_key_data in            │
│                       │                  │          │              │                   │               │ kadmin/dbutil/dump.c                                        │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2018-5709                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26458   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c           │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26458                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26461   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c   │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26461                  │
├───────────────────────┼──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│ libkrb5-3             │ CVE-2018-5709    │          │              │                   │               │ krb5: integer overflow in dbentry->n_key_data in            │
│                       │                  │          │              │                   │               │ kadmin/dbutil/dump.c                                        │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2018-5709                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26458   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c           │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26458                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26461   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c   │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26461                  │
├───────────────────────┼──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│ libkrb5support0       │ CVE-2018-5709    │          │              │                   │               │ krb5: integer overflow in dbentry->n_key_data in            │
│                       │                  │          │              │                   │               │ kadmin/dbutil/dump.c                                        │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2018-5709                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26458   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/rpc/pmap_rmt.c           │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26458                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2024-26461   │          │              │                   │               │ krb5: Memory leak at /krb5/src/lib/gssapi/krb5/k5sealv3.c   │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2024-26461                  │
├───────────────────────┼──────────────────┼──────────┤              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libncursesw6          │ CVE-2023-50495   │ MEDIUM   │              │ 6.4-4             │               │ ncurses: segmentation fault via _nc_wrap_entry()            │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2023-50495                  │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6141    │ LOW      │              │                   │               │ gnu-ncurses: ncurses Stack Buffer Overflow                  │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6141                   │
├───────────────────────┼──────────────────┼──────────┤              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libpython3.11-minimal │ CVE-2025-8194    │ HIGH     │              │ 3.11.2-6+deb12u6  │               │ cpython: Cpython infinite loop when parsing a tarfile       │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8194                   │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-4516    │ MEDIUM   │              │                   │               │ cpython: python: CPython DecodeError Handling Vulnerability │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-4516                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6069    │          │              │                   │               │ cpython: Python HTMLParser quadratic complexity             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6069                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6075    │          │              │                   │               │ python: Quadratic complexity in os.path.expandvars() with   │
│                       │                  │          │              │                   │               │ user-controlled template                                    │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6075                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-8291    │          │              │                   │               │ cpython: python: Python zipfile End of Central Directory    │
│                       │                  │          │              │                   │               │ (EOCD) Locator record offset...                             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8291                   │
├───────────────────────┼──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│ libpython3.11-stdlib  │ CVE-2025-8194    │ HIGH     │              │                   │               │ cpython: Cpython infinite loop when parsing a tarfile       │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8194                   │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-4516    │ MEDIUM   │              │                   │               │ cpython: python: CPython DecodeError Handling Vulnerability │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-4516                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6069    │          │              │                   │               │ cpython: Python HTMLParser quadratic complexity             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6069                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6075    │          │              │                   │               │ python: Quadratic complexity in os.path.expandvars() with   │
│                       │                  │          │              │                   │               │ user-controlled template                                    │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6075                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-8291    │          │              │                   │               │ cpython: python: Python zipfile End of Central Directory    │
│                       │                  │          │              │                   │               │ (EOCD) Locator record offset...                             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8291                   │
├───────────────────────┼──────────────────┼──────────┤              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libsqlite3-0          │ CVE-2025-7458    │ CRITICAL │              │ 3.40.1-2+deb12u2  │               │ sqlite: SQLite integer overflow                             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-7458                   │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-7709    │ MEDIUM   │              │                   │               │ An integer overflow exists in the FTS5                      │
│                       │                  │          │              │                   │               │ https://sqlite.org/fts5.html e ...                          │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-7709                   │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2021-45346   │ LOW      │              │                   │               │ sqlite: crafted SQL query allows a malicious user to obtain │
│                       │                  │          │              │                   │               │ sensitive information...                                    │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2021-45346                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-29088   │          │              │                   │               │ sqlite: Denial of Service in SQLite                         │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-29088                  │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-52099   │          │              │                   │               │ sqlite: SQLite integer overflow                             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-52099                  │
├───────────────────────┼──────────────────┤          │              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libssl3               │ CVE-2025-27587   │          │              │ 3.0.17-1~deb12u3  │               │ OpenSSL 3.0.0 through 3.3.2 on the PowerPC architecture is  │
│                       │                  │          │              │                   │               │ vulnerable ......                                           │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-27587                  │
├───────────────────────┼──────────────────┤          │              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libstdc++6            │ CVE-2022-27943   │          │              │ 12.2.0-14+deb12u1 │               │ binutils: libiberty/rust-demangle.c in GNU GCC 11.2 allows  │
│                       │                  │          │              │                   │               │ stack exhaustion in demangle_const                          │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2022-27943                  │
├───────────────────────┼──────────────────┼──────────┤              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libtinfo6             │ CVE-2023-50495   │ MEDIUM   │              │ 6.4-4             │               │ ncurses: segmentation fault via _nc_wrap_entry()            │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2023-50495                  │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6141    │ LOW      │              │                   │               │ gnu-ncurses: ncurses Stack Buffer Overflow                  │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6141                   │
├───────────────────────┼──────────────────┤          │              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ libuuid1              │ CVE-2022-0563    │          │              │ 2.38.1-5+deb12u3  │               │ util-linux: partial disclosure of arbitrary files in chfn   │
│                       │                  │          │              │                   │               │ and chsh when compiled...                                   │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2022-0563                   │
├───────────────────────┼──────────────────┼──────────┤              ├───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ python3.11-minimal    │ CVE-2025-8194    │ HIGH     │              │ 3.11.2-6+deb12u6  │               │ cpython: Cpython infinite loop when parsing a tarfile       │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8194                   │
│                       ├──────────────────┼──────────┤              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-4516    │ MEDIUM   │              │                   │               │ cpython: python: CPython DecodeError Handling Vulnerability │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-4516                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6069    │          │              │                   │               │ cpython: Python HTMLParser quadratic complexity             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6069                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-6075    │          │              │                   │               │ python: Quadratic complexity in os.path.expandvars() with   │
│                       │                  │          │              │                   │               │ user-controlled template                                    │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-6075                   │
│                       ├──────────────────┤          │              │                   ├───────────────┼─────────────────────────────────────────────────────────────┤
│                       │ CVE-2025-8291    │          │              │                   │               │ cpython: python: Python zipfile End of Central Directory    │
│                       │                  │          │              │                   │               │ (EOCD) Locator record offset...                             │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8291                   │
├───────────────────────┼──────────────────┼──────────┼──────────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ zlib1g                │ CVE-2023-45853   │ CRITICAL │ will_not_fix │ 1:1.2.13.dfsg-1   │               │ zlib: integer overflow and resultant heap-based buffer      │
│                       │                  │          │              │                   │               │ overflow in zipOpenNewFileInZip4_6                          │
│                       │                  │          │              │                   │               │ https://avd.aquasec.com/nvd/cve-2023-45853                  │
└───────────────────────┴──────────────────┴──────────┴──────────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────────────┘

Python (python-pkg)

Total: 1 (UNKNOWN: 0, LOW: 0, MEDIUM: 1, HIGH: 0, CRITICAL: 0)

┌────────────────┬───────────────┬──────────┬────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────┐
│    Library     │ Vulnerability │ Severity │ Status │ Installed Version │ Fixed Version │                        Title                        │
├────────────────┼───────────────┼──────────┼────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────┤
│ pip (METADATA) │ CVE-2025-8869 │ MEDIUM   │ fixed  │ 25.2              │ 25.3          │ pip: pip missing checks on symbolic link extraction │
│                │               │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2025-8869           │
└────────────────┴───────────────┴──────────┴────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────┘
```
