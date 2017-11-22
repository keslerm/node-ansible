ansible-wrapper
============

Programmatic interface in Node.js for executing Ansible ad-hoc commands and playbooks

#### Warning: this package is still under development. API might break between minors.

### Installation

`npm install ansible-wrapper --save`

**NOTE:** I think it goes without saying, but I'll mention it anyway - you MUST have ansible installed on the same machine on which your node process is going to run.


### Crash Course

```javascript
var Ansible = require('ansible-wrapper');
var command = new Ansible.AdHoc().module('shell').hosts('local').args("echo 'hello'");
command.exec();
```

is equivalent to:

```shell
ansible local -m shell -a "echo 'hello'"
```

```javascript
var playbook = new Ansible.Playbook().playbook('my-playbook');
playbook.exec();
```

is equivalent to:

```shell
ansible-playbook myplaybook.yml
```

Let's execute:

```javascript
var promise = playbook.exec();
promise.then(function(successResult) {
  console.log(successResult.code); // Exit code of the executed command
  console.log(successResult.output) // Standard output/error of the executed command
}, function(error) {
  console.error(error);
})
```

We can also get the results of a command streamed in real time (from both playbooks and adhoc commands):
```javascript
playbook.on('stdout', function(data) { console.log(data.toString()); });
playbook.on('stderr', function(data) { console.log(data.toString()); });
var promise = playbook.exec();
```

[Full Documentation](http://shaharke.github.io/ansible-wrapper)

### Running tests:

`npm test`

### License

[MIT](https://github.com/keslerm/ansible-wrapper/blob/master/LICENSE)
