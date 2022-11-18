# SparrowCI pipelines for everything

New year and Christmas is a fun time when the whole family gets together by dinner table and have some good time.

Let me introduce some fun and young member of a Raku family called SparrowCI - super flexible and fun to use CI service.

To visit SparrowCI - got https://ci.sparrowhub.io web site and get a login using your GitHub credentials:

![login](https://raw.githubusercontent.com/melezhik/advent/master/images/sparrowci/login.jpeg)

# Pipelines of fun

SparrowCI provides you with some DSL to built a CI for your Raku modules. 

For none devops people CI - means continues integration - a process allowing to test your code in some centralized 
service and share results with others.

Let's create some Christmas gift module and then build it:


```bash
mkdir SparrowBird
cd SparrowBird
git init 
cat << HERE > META6.json
{
  "authors" : [
    "Alexey Melezhik"
  ],
  "description" : "Sparrow::Bird",
  "license" : "Artistic-2.0",
  "name" : "Sparrow Bird",
  "provides" : {
    "Teddy::Bear" : "lib/Sparrow/Bird.pm6"
  },
  "version" : "0.0.1"
}
HERE

mkdir -p lib/Sparrow

cat << HERE > lib/Sparrow/Bird.rakumod
unit module Sparrow::Bird;
HERE


```

The add some SparrowCI pipeline to build a project:

```bash
cat << HERE > sparrow.yaml
tasks:
  -
    name: build-sparrow
    default: true
    language: Bash
    code: |
      set -e
      cd source/
      zef test .
HERE
```

And finally let's commit everything:

```bash
git add .
git commit -a -m "my sparrow bird module initial commit"

git remote add origin git@github.com:melezhik/sparrow-bird.git
git branch -M main
git push -u origin main
```

Once a module in a GitHub, let's register it in SparrowCI:

Go to "my repos", then a repository you want to build - https://github.com/melezhik/sparrow-bird :

![add repo](https://raw.githubusercontent.com/melezhik/advent/master/images/sparrowci/add-repo.png)

# The Gift

Now it's time to see the very first build, please allow SparrowCI a minute as it being very busy at this time
sending Christmas gifts to all it's users. So we will see something like this:

![report](https://raw.githubusercontent.com/melezhik/advent/master/images/sparrowci/report.jpeg)


# That is it?

Well it's small new year story not pretending to do a boring documentation stuff. But as the title say - SparrowCI pipelines for _everything_, 
not just for building Raku modules ...

Checkout https://ci.sparrowhub.io to see all fun SparrowCI features and happy holidays!


