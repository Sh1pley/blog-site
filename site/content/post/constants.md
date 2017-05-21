+++
date = "2017-01-03T14:21:26-06:00"
draft = false
title = "Constants"
description = "Ruby's way or the highway!"
+++

#### "One man's constant is another man's variable."
###### -Alan Perlis
---
---
## Constant 
##### from Google
*From late Middle English (in the sense ‘staying resolute or faithful’): from Old French, from Latin constant- ‘standing firm,’ from the verb constare, from con- ‘with’ + stare ‘stand.’ The noun senses date from the mid 19th century.*
#### Adj: occurring continuously over a period of time.
#### Noun: a situation or state of affairs that does not change.
---
## Ruby's Definition:
#### A constant has a name starting with an uppercase character. It should be assigned a value at most once. In the current implementation of ruby, reassignment of a constant generates a warning but not an error (the non-ANSI version of eval.rb does not report the warning):
---
So between the two defiitions, we see some firm rules:

- A Constant has a name.
- A constant's name begins with an uppercase character/or all uppercase characters.

Through practice we can see it in action:
![definining/calling a constant](/images/content/constant/Constant1.png)
Easy enough, can define your data in the constant and use namespacing to recall it. Nice.

And then this '_should_' rule:

- It should be assigned a value at most once..

In practice:
![altering a constant](/images/content/constant/Constant2.png)
Warning: '.. previous version of constant was here..' Ok. You get a message and it still takes the change.
Not constant anymore..

Another thing:
![How about a set of data, and Array](/images/content/constant/Constant3.png)
No warning message.. Altered the constant anyway. If done on purpose, great.
On accident.. Not so great. Probably gonna get some awesome errors. Probably.

More things:
![.pop](/images/content/constant/Constant4.png)
Pop/Shift/Unshift all the other things. Will do what you expect them to do.
And the best part, no warnings. Ruby gives you the power, lets you get as weird as you want.

So this is where you begin to agree or disagree to Ruby's terms of service. And either start smelling good.
Or smelling bad. This is also where your code can get real messy. Real fast.

This is also where testing your Models; making sure that everytime you alter and manipulate your data
that your doing it on purpose. With results that you are expecting.

Ruby kind of just leaves the decision making up to you. Although it is nice enough to give you tools to help in your travels.
In comes Freeze: 
##### from the Ruby Docs
#### freeze → obj 
#### Prevents further modifications to obj. A RuntimeError will be raised if modification is attempted. There is no way to unfreeze a frozen object. See also Object#frozen?.
#### This method returns self.

In practice: 
![.freeze](/images/content/constant/Constant5.png)
This becomes my personal expected behavior. Allows you to define and call your constant set of date.
And any of the weird stuff you want to try and do to said data, gives you a nice error that says nope.

Nope. The best kind of ruby error. Just a good ol fashioned stop it, you can't do this.
From here you can say "Thanks Ruby. I didn't know you couldn't do that." But you did.
Because you had to warn Ruby that you wanted to be told no if you were trying to get weird.
