# SparrowCI pipelines cascades of fun


Remember the young in the previous SparrowCI story? We have not finished with him yet ...
Because new year time is coming and brings us a lot of fun, or we can say
cascades of fun ...

So, our awesome SparrowCI pipelines plumber guy is busy with sending a git to his nephew:

`sparrow.yaml`:

```yaml
tasks:
  -
    name: zef-build
    language: Bash
    default: true
    code: |
      set -e
      cd source/
      zef install --deps-only --/test .
      zef test .
```

Once a gift is [packed](https://ci.sparrowhub.io/report/1919) and ready, there is one little thing that is left.

\- And that is - send the gift to a Santa, to His wonderful [(Raku|LAP)land](https://raku.land)

So SparrowCI guys get quickly to, it as he knows who he does ( did not I tell you ,
he is very knowledgeable? :-), and create a nifty script to publish things to 
Santa's land:

`.sparrow/publish.yaml`

```yaml
image:
  - melezhik/sparrow:debian

secrets:
  - FEZ_TOKEN
tasks:
  - name: fez-upload
    default: true
    language: Raku
    init: |
      if config()<tasks><git-commit><state><comment> ~~ /'Happy New Year'/ {
        run_task "upload"
      }
    subtasks:
    -
      name: upload
      language: Bash
      code: |
        set -e
        cat << HERE > ~/.fez-config.json
          {"groups":[],"un":"melezhik","key":"$FEZ_TOKEN"}
        HERE
        cd source/
        zef install --/test fez
        head Changes
        tom --clean
        fez upload
    depends:
      -
        name: git-commit
  - name: git-commit
    plugin: git-commit-data
    config:
      dir: source
```

Of course to publish a package in Santa's store, SparrowCI lad needs to tell Santa's his secret 
\- aka fez token, but don't worry, Santa knows how to keep your secrets!

![secret](https://raw.githubusercontent.com/melezhik/advent/master/images/sparrowci/secret.png)


Finally, SparrowCI [ties](https://github.com/melezhik/rakudist-teddy-bear/blob/master/sparrow.yaml) 
"package" with "publish":

`sparrow.yaml`:

```yaml
# ...
followup_job: .sparrow/publish.yaml
```

... And, it's time to share some gifts:


```bash
git commit -m "Happy New Year" -a
git push
```

Remember, what should we say to Santa, once we see him? Yes - Happy New Year!

This "magic" commit phrase will make our package be delivered to Santa's land:
