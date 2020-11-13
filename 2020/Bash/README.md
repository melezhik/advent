# Mixing Bash and Raku Using Sparrow

[Sparrow](https://github.com/melezhik/Sparrow6) is a Raku automation framework which could be easily
integrated with many programming languages. So if you come from none Raku language - you're welcome.

In this post I'll show you have one can effectively mix Bash scripts and Raku language using Sparrow.

The idea of Sparrow - to choose the language that fits best to your domain and let Raku orchestrate your code on high level.

Let's get started.

---

# Create a Bash task

Bash task should be just a script named `task.bash`:

`$ nano task.bash`

```bash

echo "hello from Bash"

```

Now let's run it using Sparrow:

`s6 --task-run .`

```
$ s6 --task-run .
[sparrowtask] :: run sparrow task .
[sparrowtask] :: run thing .
[.] :: hello from Bash
```

Of course it'd be silly to stop here, what is the reason to run Bash script through Sparrow rather then running it directly through Bash?

Here some benefits Sparrow gives you

# Handling input parameters

Say, we want to pass some input parameters to our Bash task. Handling input parameters with Bash is not an easy task, but not with Sparrow:


```bash
echo "hello from $(config language)"
```

Now we can pass a parameter:

```
$ s6 --task-run .@language=Bash
[sparrowtask] :: run sparrow task .@language=Bash
[sparrowtask] :: run thing .
[.] :: hello from Bash
```

We can even define a default value for a language parameter:

`$ nano config.yaml`

```yaml
language: Shell
```

```
$ s6 --task-run .
[sparrowtask] :: run sparrow task .
[sparrowtask] :: run thing .
[.] :: hello from Shell
```

To pass multiple parameters through a command line, use `,` delimiter:

```
$ s6 --task-run .@language=Raku,version=2020.10
```

# Running Bash as a Raku function

What makes a usage of Sparrow really cool is that one can run the same Bash task as Raku function.

Let's create a Raku scenario which is an equivalent to the command line run above:

`$ nano run.raku`

```
use Sparrow6::DSL;
task-run ".", %(
  language => "Raku"
);
```

```
$ raku run.raku 
[.] :: hello from Raku
```

# Checking STDOUT

Sometimes when I write test scripts, I'd like to verify that script's output has some lines. The same way Linux people would use `| grep` construction.

In Sparrow this is an embedded feature and called task checks:


`$ nano task.check`

```
regexp: Bash || Shell || Perl || Python || Raku
```

```
raku  run.raku 
[.] :: hello from Raku
[task check] stdout match <Bash || Shell || Perl || Python || Raku> True
```

As you could guess, task check is a file containing regular expressions ( not only )  to check task stdout. 

In case a check fails, Sparrow will notify you and throw an exception:

(Sorry, Basic language users)

```
$ s6 --task-run .@language=Basic
[.] :: hello from Basic
[task check] stdout match <Bash || Shell || Perl || Python || Raku> False
=================
TASK CHECK FAIL

$ echo $?
2
```

# Pre run hooks

Like we git system when users can define some scripts gets run _before_ they do commits or send changes to a server, Sparrow allows
hooks when you write your scripts. 

Say, you want to run _another_ Bash script before you run a _main_ one:

`$ mkdir tasks/triggers/http`

`$ nano tasks/triggers/http/task.bash`

```bash
curl 127.0.0.1:3000 -o /dev/null -s && echo "I am triggered"
```

`$ nano hook.bash`

```bash
run_task triggers/http
```

```
$ s6 --task-run .
[sparrowtask] :: run sparrow task .
[sparrowtask] :: run thing .
[.] :: I am triggered
[.] :: hello from Shell
[task check] stdout match <Bash || Shell || Perl || Python || Raku> True
```

In this example we hit some URL and print out "I am triggered" on success before we run main task.

It's even possible to pass parameters between tasks:

`$ nano hook.bash`

```bash
run_task triggers/http param value
```

`$ nano tasks/triggers/http/task.bash`

```bash
curl 127.0.0.1:3000 -o /dev/null -s && echo "I am triggered. You passed me param: $param"
```

```
$ s6 --task-run .
[sparrowtask] :: run sparrow task .
[sparrowtask] :: run thing .
[.] :: I am triggered. You passed me param: value
[.] :: hello from Shell
[task check] stdout match <Bash || Shell || Perl || Python || Raku> True
```

# Packaging things up

And the last killer feature of Sparrow is one can distribute their scripts, using Sparrow plugins mechanism. It's just easy as
creating a package meta file for your scripts:


`$ nano sparrow.json`

```json
{
  "name" : "hello-language",
  "version" : "0.0.1",
  "description" : "hello language plugin"
}
```

```
$ s6 --upload
[repository] :: upload plugin
[repository] :: upload hello-language@0.0.1
```

Now your mates could reuse the plugin:


```
$ s6 --index-update
[repository] :: update local index
[repository] :: index updated from file://home/melezhik/repo/api/v1/index
```

Through a command line:

```
$ s6 --plg-run hello-language@language=Python
[repository] :: install plugin hello-language
[repository] :: installing hello-language, version 0.000001
[task] :: run plg hello-language@language=Python
[task] :: run thing hello-language
[hello-language] :: I am triggered. You passed me param: value
[hello-language] :: hello from Python
[task check] stdout match <Bash || Shell || Perl || Python || Raku> True
```

Or as Raku API:

```raku
task-run "hello language", "hello-language", %(
  language => "Raku"
)
```

```
[hello language] :: I am triggered. You passed me param: value
[hello language] :: hello from Raku
[task check] stdout match <Bash || Shell || Perl || Python || Raku> True
```

Sparrow supports various protocols for scripts distribution, including http, https, rsync and ftp.

One can find a lot of plugin examples at [sparrowhub.io](https://sparrowhub.io/) - Sparrow plugins repository.

# Conclusion

As one can see with reasonable amount of Raku code we can develop and manage Bash scripts efficiently.

Doing in Bash style the part of job where Bash fits best while leaving high level orchestration to Raku.


