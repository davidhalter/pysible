Beautiful Pythonic Ansible (RFC)
================================

Ansible's declarative style makes it very easy to do configuration management.
While the idea of ansible is a pretty good one, its YAML playbooks often
confuse people. With the addition of blocks in Ansible 5.3, Ansible is looking
more and more like a programming language.

However, readability is key and Ansible sometimes fails to deliver the user
experience you would like to. Imagine a much simpler workflow. Instead of:

.. code-block:: yaml

    - name: Clone git repositories
      git: repo={{ item.repo }}
           dest={{ item.path }}
           clone=yes
           update=yes
      register: repository_update
      with_items:
        - { repo: "{{ repo1 }}", path: "/foo" }
        - { repo: "{{ repo2 }}", path: "/bar" }

    - name: Next task

you could just write:

.. code-block:: yaml

    from pysible.modules import git

    # Clone git repositories
    for repo, dest in [(REPO, '/foo'), (REPO2, '/bar')]:
        last_update = git(repo=REPO, dest=REPO, clone=True, update=True)
    
    # Next task


This allows for a lot more flexibility. Instead of includes you use simple
functions and instead of roles... Well don't even get me started on that. I
think that instead of using a pseudo language in YAML, using Python has the
advantage of clean proper coding with all the advantages a language like Python
comes in. Additionally we don't really have to give up Ansible features. We can
use handlers, includes and modules like before. Just with a slightly different
API.

It would be a brave new world, but finally there would be beautiful deployment
code!

Running would be as simple as running Ansible. A simple command `pysible
<playbook> -i <hosts-file>` would be enough to trigger execution on a few
hosts.  This command would ensure that host files could still be used
(including host/group variables).


Implementation
--------------

At the moment there is a proof of concept that I won't publish, yet. However if
you would like to see this, start a conversation in the issue tracker or star
the repository.

The current implementation is merely 30 lines long, because I'm using ansible's
internal APIs. I'm thinking about publishing this tool.
