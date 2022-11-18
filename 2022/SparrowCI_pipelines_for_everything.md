# SparrowCI pipelines for everything

New year and Christmas is a fun time when the whole family gets together by dinner table and have some good time.

Let me introduce some fun and young member of a Raku family called SparrowCI - super flexible and fun to use CI service.

To visit SparrowCI - got https://ci.sparrowhub.io web site and get a login using your GitHub credentials.

# Pipelines of fun

SparrowCI provides you with some DSL to built a CI for your Raku modules. 

For none devops people CI - mean continues interation - a process allowing to test your code in some centralaized 
service and share results with others.

Let's create some Christma gift module and then build it:



```bash
mkdir SparrowBird
cd SparrowBird
git iit
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
git add META6.json
mkdir -p lib/Sparrow

cat << HERE > lib/Sparrow/Bird.rakumod
unit module Sparrow::Bird;
```

Once a module in a GitHub, let's register it in SparrowCI:

Go to "my repos", then a repository you want to build - https://github.com/melezhik/sparrow-bird
