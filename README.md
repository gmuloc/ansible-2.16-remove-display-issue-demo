# Reproducing issue https://github.com/ansible/ansible/issues/82358

cf context in the issue

## Run with ansible 2.15.7 and this will completed as follow

```
ansible-playbook playbook.yml
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Test] **********************************************************************************************************************************************************************************************************************************

TASK [Running module] ************************************************************************************************************************************************************************************************************************
fatal: [localhost]: FAILED! => {"msg": "[DEPRECATED]: Foo. This feature was removed in a future release. Please update your playbooks."}

PLAY RECAP ***********************************************************************************************************************************************************************************************************************************
localhost                  : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```

## Run with ansible >= 2.16 and this will hang as follow

```
ansible-playbook playbook.yml
[WARNING]: No inventory was parsed, only implicit localhost is available
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Test] **********************************************************************************************************************************************************************************************************************************

TASK [Running module] ************************************************************************************************************************************************************************************************************************
Exception in thread Thread-1 (results_thread_main):
Traceback (most recent call last):
  File "/Users/guillaumemulocher/.pyenv/versions/3.10.4/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/threading.py", line 1009, in _bootstrap_inner
    self.run()
  File "/Users/guillaumemulocher/.pyenv/versions/3.10.4/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/threading.py", line 946, in run
    self._target(*self._args, **self._kwargs)
  File "/Users/guillaumemulocher/.pyenv/versions/3.10.4/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/ansible/plugins/strategy/__init__.py", line 120, in results_thread_main
    dmethod(*result.args, **result.kwargs)
  File "/Users/guillaumemulocher/.pyenv/versions/3.10.4/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/ansible/utils/display.py", line 134, in proxyit
    return method(self, *args, **kwargs)
  File "/Users/guillaumemulocher/.pyenv/versions/3.10.4/Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/ansible/utils/display.py", line 519, in deprecated
    raise AnsibleError(message_text)
ansible.errors.AnsibleError: [DEPRECATED]: Foo. This feature was removed in a future release. Please update your playbooks.
```

You have to interrupt with Ctrl+C
