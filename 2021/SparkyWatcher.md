# Synopsis

Sparky Watcher Jobs - Asynchronous Jobs in Sparky CI Server.

# Welcome Sparky

Sparky - is a pocket size, minimalist yet powerful CI server written on Raku.

I use Sparky on my daily @job to run various automation tasks.

Recent Sparky release has a new feature called watcher jobs.

In nutshell watcher jobs allow one to run multiple jobs and wait

when all they are finished. 

Ready to dive in?


# Santa in hurry to send you a gift

So holiday season is approaching and Santa is busy, he wants to
send all his gifts all around the world. 

Elfs are helping him:


`hosts.raku`:

```raku
[
  %(
    host => "localhost",
    tags => %(
      country => "Russia",
      greeting => "С Новым Годом",
      name => "HappyNewYearRussia",
    ),
  ),
  %(
    host => "localhost",
    tags => %(
      country => "Finland",
      greeting => "Hyvää Uutta Vuotta!" 
      name => "HappyNewYearFinland",
    ),
  ),
  %(
    host => "localhost",
    tags => %(
      country => "US",
      greeting => "Happy New Year"
      name => "HappyNewYearUS",
    ),
  )
]
```
