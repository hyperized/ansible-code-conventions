# ansible-code-conventions
A guide on how to write effective and fault tolerant Ansible code.

Ansible code convention:

- Descriptions are not capitalized
- The indentation is 2 (two) spaces, for vim you can use [vim-ansible-yaml](https://github.com/chase/vim-ansible-yaml)
- Use dict notation, not in-line, this prevents errors later on, example:

```yaml
bad: this=command

good:
  this: command
```
- When a module does require '=' parameters, use the following:

```yaml
moduleX:
  args:
    var: 'something'
```
- When a module does call something using '=' parameters, use the following:

```yaml
moduleX: >
  var='something'
```
- When dealing with multiple `when` conditions:

```
- name: do task
  command: echo hello world!
  when:
    - (skip_install != True)
    - (ansible_os_family == 'Debian')
    - (command_result.changed == True)
```

- Playbooks should be used for role assignments.
	- Use tagging to call a role:
```
roles:
  - { role: myrole, tags: ['myrole', 'optional_alias_myrole'] }
```
- Roles should be used for functional blocks.
    - All templates must contain a `{{ ansible_managed }}` header.
    - Ensure `ansible_managed` header is statically set in `ansible.cfg` or it breaks idempotency.
	- Every role has a self documenting `defaults/main.yml` file that contains all set-able variables.
	- All variables in the role scope must be named `role_variable` with the option of spaces being replaced by `_` hyphens.
	- The use of dict style variable settings `role.variable` is discouraged as it complicates overriding.
	- Every variable on a new line for readability.
	- Role must be a complete basic implementation without requiring further variables.
	- Role must be idempotent, meaning: including the role must result in OK, not changed or error when nothing has been changed on the target host.
	- Try to stow everything in `main.yml`, only use include when it makes sense, which is:
		- when you need to support multiple platforms, then include each in `main.yml`
	- Use of comments indicate bad code.
- Completeness (especially idempotency) on the target platform is preferred over portability when starting out a new role.
- Don't use: `backup: yes`, rely on git instead!
