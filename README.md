Sample project for playbook_dir is not found for includes
=========================================================

Output of `ansible-playbook -i hosts -c local playbook.yml` with 1.7.2
=======================================================================

```
    PLAY [Show playbook_dir] ****************************************************** 

    GATHERING FACTS *************************************************************** 
    ok: [127.0.0.1]

    TASK: [a | Do stuff] ********************************************************** 
    ok: [127.0.0.1] => {
        "msg": "Hallo"
    }

    TASK: [a | Do stuff in a::foo] ************************************************ 
    ok: [127.0.0.1] => {
        "playbook_dir": "/Users/mifr/workspace/foss/ansible-no-playbookdir"
    }

    TASK: [b | Do stuff in a::foo] ************************************************ 
    ok: [127.0.0.1] => {
        "playbook_dir": "/Users/mifr/workspace/foss/ansible-no-playbookdir"
    }

    TASK: [debug var=playbook_dir] ************************************************ 
    ok: [127.0.0.1] => {
        "playbook_dir": "/Users/mifr/workspace/foss/ansible-no-playbookdir"
    }

    PLAY RECAP ******************************************************************** 
    127.0.0.1                  : ok=5    changed=0    unreachable=0    failed=0   
```

Reproduce with 1.8 devel (19606afe5f)
=====================================
Given you checked out `ansible` to `../ansible` *and* initialized submodules run:

```
    virtualenv -p /usr/bin/python venv
    . venv/bin/activate/
    pip install ../ansible # to get dependencies
    . ../ansible/hacking/env-setup
    ansible-playbook --version
    ansible-playbook 1.8 (devel 19606afe5f) last updated 2014/11/25 08:20:50 (GMT +200)
        lib/ansible/modules/core: (detached HEAD fbc4ed7a88) last updated 2014/11/25 09:29:11 (GMT +200)
        lib/ansible/modules/extras: (detached HEAD e64751b0eb) last updated 2014/11/25 09:29:25 (GMT +200)
        v2/ansible/modules/core: (detached HEAD cb69744bce) last updated 2014/11/25 09:28:25 (GMT +200)
        v2/ansible/modules/extras: (detached HEAD 8a4f07eecd) last updated 2014/11/25 09:28:36 (GMT +200)
        configured module search path = None
```

Running: `ansible-playbook -i hosts -c local playbook.yml` now ends with:

```
    ERROR: file could not read: /Users/mifr/workspace/foss/ansible-no-playbookdir/roles/b/tasks/{{playbook_dir}}/roles/a/tasks/foo.yml
``

Environment
===========

* OS: Mac OS X 10.10.1

```
    (venv)mifr@HOST:ansible-no-playbookdir$ which python
    /Users/mifr/workspace/foss/ansible-no-playbookdir/venv/bin/python
    (venv)mifr@HOST:ansible-no-playbookdir$ python 
    Python 2.7.6 (default, Sep  9 2014, 15:04:36) 
    [GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.39)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 
```



