# Lab 1 submission

Fill in every line marked `ANSWER:` (one to three sentences each). Write the answer on the same line, right after `ANSWER:`; for pasted output, add a short summary there and paste the output below. Push before the start of the week 3 session.

## Part A - targets

- Path used (azure or local): ANSWER:
- If Azure: region used: ANSWER:

## Part B - inventory and ad hoc commands

1. Paste the output of `ansible-inventory --graph`.
   ANSWER:group web contains web1 and web2, and both inherit http_port = 80.

```
@all:
  |--@ungrouped:
  |--@web:
  |  |--web1
  |  |--web2
```

2. You ran `ansible web -b -m ansible.builtin.apt -a "name=htop state=present update_cache=true"` twice. What did each run report (changed or ok), and why?
   ANSWER: The first run reported changed because htop was not installed yet and apt installed it. The second run reported ok (changed=false) because htop was already present, so state=present had nothing to do.

3. What is the difference between `ansible web -a "uptime"` and `ansible web -m ping`? Which one checks that Python works on the target?
   ANSWER:`-a "uptime"` runs a shell command and returns its output, while `-m ping` runs the ping module, which only confirms that SSH works and that Ansible can run a Python module on the target (it replies "pong"). `ping` is the one that checks that Python works.

## Part C - YAML

4. Which of the six problems in `yaml/shop.yml` would have broken a real deployment silently, without any error message?
   ANSWER: The three that load without error but give wrong values: `country: NO` is read as the boolean false, `version: 1.10` is read as the number 1.1, and the duplicate `enabled` key silently keeps only the last value. The other three (tab, extra colons in tagline, bad indentation of email) produce a parse error.

## Part D - first playbook

5. Paste the PLAY RECAP of your first run and of your second run.
   ANSWER:first run changed=2 on each host (home page and service), second run changed=0 on each host.
First run:
web1 : ok=4 changed=2 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0
web2 : ok=4 changed=2 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0

Second run:
web1 : ok=4 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0
web2 : ok=4 changed=0 unreachable=0 failed=0 skipped=0 rescued=0 ignored=0

6. Why does `cache_valid_time: 3600` matter for idempotency?
   ANSWER: Without it, update_cache: true makes apt refresh the package cache on every run, which is an unnecessary action that can be reported as a change. With cache_valid_time: 3600, apt skips the refresh if the cache is less than an hour old, so a second run changes nothing.


## Cleanup

- Azure resource group deleted, or local containers removed (yes/no): ANSWER:yes
