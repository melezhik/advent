# Mixing Bash and Raku Using Sparrow

Today I'd like to show you have one can mix Bash scripts and Raku language using Sparrow framework.

# Create a Bash task

Bash task should be just a script named `task.bash`:

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
language: Bash
```
