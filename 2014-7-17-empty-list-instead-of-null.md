---
layout: post
title: "You Should Return an Empty List Instead of Null"
comments: false
description: "These are all the commands you'll need to use vim macros.."
---

Time for vegetables. In this post, we'll discuss how to improve code style, avoid bugs, and improve clarity.

Occasionally, when reading code we stumble upon things that feel awfully peculiar. Let's mock out a situation by putting ourselves in the shoes of a dark goblin overlord demanding control of the realm. Some elves, humans, dwarves, or whatever are becoming an utter nuisance for us. "What a pain", we say to ourselves as we begin to summon the armies we control with a GoblinHorde class:

    class GoblinHorde {
      private List<Goblin> goblins;
    
      public GoblinHorde() {
        goblins = new ArrayList<>();
      }
    
      public List<Goblin> gatherGoblinArmy() {
        if (goblins.empty()) {
          return null;
        }
    
        List<Goblin> gatheredGoblins = new ArrayList<>();
        for (Goblin wobblyGobbly : goblins) {
          if (wobblyGobbly.isReadyForBattle() {
            gatheredGoblins.add(wobblyGobbly);
          }
        }
        return gatheredGoblins;
      }
    
      public void addGoblin(Goblin goblin) {
        goblins.add(goblin);
      }
    }

Okay, can you spot the weird piece? I mean, this is all at least a little weird, but can you spot the offender...

...

...

        if (goblins.empty()) {
          return null;
        }

OH OUCH wooch owwie! This actually happens in code that people write. A less obvious flavor of this offense would be to defer initializing goblins when a goblin is first added and if no goblin is added when you try to gather your goblin army, we pass back the uninitialized goblins when they are requested. It would looks something like this:

    class GoblinHorde {
      private List<Goblin> goblins;

      public List<Goblin> gatherGoblinArmy() {
        if (goblins == null) {
          return null;
        }

        List<Goblin> gatheredGoblins = new ArrayList<>();
        for (Goblin wobblyGobbly : goblins) {
          if (wobblyGobbly.isReadyForBattle() {
            gatheredGoblins.add(wobblyGobbly);
          }
        }
        return gatheredGoblins;
      }

      public void addGoblin(Goblin goblin) {
        if (goblins == null) {
          goblins = new ArrayList<>();
        }
        goblins.add(goblin);
      }
    }

You and your evil council might be saying to yourselves, "oh this feels, a little better - at least we're not wasting any space on pesky Lists that we aren't using." While this is somewhat true, it's still no good for two reasons.

* You create many opportunities to throw NullPointerExceptions. Many of which can be subtle and refuse to present themselves until rare conditions are met.
* You're code becomes littered with: 

    List<Goblin> goblins = horde.gatherGoblins()
    if (goblins != null && goblins.size() > 10) {
      beMenacingWith(goblins);
    }

    // or yick still
    if (goblins != null) {
      for (Goblin globlin : goblins) { //intentional - its fun to say out loud.
        sendOfToBattle(globlin);
      }
    }

In the case, if (goblins != null) does little to reveal your intentions and will not help the next reader of your code understand what is going on. You may think you are asking if there are any goblins, but you could be asking if something has gone wrong with the collection of goblins, or if there is such a thing as a collection of goblins. It feels noisy and gets interferes with your ability to explain what you really intend to do.

Eric Lippert provides a [very lucid treatment of nulls](http://blogs.msdn.com/b/ericlippert/archive/2009/05/14/null-is-not-empty.aspx).

You may think it is wasteful to allocate a list where it is not needed. This belief has two problems. First, one should focus on writing clear code before you worry about optimization. This is not meant as encouragement to choose bad algorithms. It's just that maintainability is an important fight to always fight and [optimization should be handled on an a la carte basis](http://c2.com/cgi/wiki?PrematureOptimization). You only have so much time to work - the time you save by making your code clear and easy to understand can go to fixing a real bottleneck or implementing important features. Really consider the costs and the benefits.

The second, and less opinion driven argument is that there really is very little allocation cost at all. You can simply return Collections.emptyList() where you would otherwise return null. emptyList() returns the same immutable copy of an empty list every time it is called. That means the cost of calling it is constant and incredibly small.

Good luck with the realm and If you are an offender, please reform your ways : )