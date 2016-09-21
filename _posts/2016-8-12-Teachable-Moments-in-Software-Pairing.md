---
layout: post
title: Teachable Moments in Software Pairing
---

My last week at CoverMyMeds was a lot of fun. It was satisfying to have no
loose ends left untied. One of the benefits was being able to
work with a co-op on our team, Braden. Braden started a few months ago and
pairing in the office made me wish that I had spent more time pairing with him
since he started. Pairing with Braden reminded me of what it was like when I
first started, deer in the headlights, confused about everything going on. The
inspiration for this post started when he asked me to review his code, and that
led me down this rabbit hole.

It's pretty typical to get a hipchat message asking to have me review someone's
code. I'm sure this happens to most developers. We look at it, send back our
comments, start a conversation, and inevitably, give it a `:+1:`. I was going to
do that again today; but, I got in early, I didn't have much on my agenda, I
told Braden I was going to review it with him. This allowed me to do more than
just review his code. I could give him insights on what I think about when
reviewing. What kinds of questions I ask myself, like "could this be
simplified?", "how clear and readable is this code?", "if this test failed,
would I be able to figure out what went wrong?", "are there any cases that don't
seem to be tested but should be?" 

As we were walking through his code, I could tell that there were some points of
weakness that could be strengthened. When I noticed these, I stopped what we
were doing. Reviewing code was nowhere near important as this teachable moment.
It was very trivial. Should this method test be describing `#open` or `.open`?
There's a 50/50 chance of getting this right, and luckily for Braden, he got it
wrong. The `Chat.open` method was a scope listed on this model. Unfortunately, I
don't have the code available to me anymore, but it looked something like this:
```
scope :open, -> { where(active: true) }
```

This was a great example. We quickly moved to a blank text document and I asked
to take the Chat model and remove everything except this line. From here, I
asked him to write the ruby code that this is a shortcut for. He looked at it,
and couldn't figure it out. 

So we broke it down. 

How would we call this? He looked for a little and ended up with: `Chat.open()`

Which is correct. So then I asked, how would you write that method?

```ruby
def self.open
self.where(active: true)
end
```

Great, we had written it out. Now I moved from intermediate to advanced, how
would we rewrite the `.scope` method? 

This took a while as well. We had to break it down. What is calling scope in the
middle of this line actually doing? Well it's just calling a method called
`scope` where we pass in 2 arguments, a symbol, and a block. So the method
declaration became easy. 
```ruby
def scope(method_name, method_call)
 #blah
end
```
Where I had to explain the concept of _metaprogramming_. Something that I hadn't
touched until months after starting, and Braden was getting a taste in just a 
few weeks. Once we talked about `define_method`, it was easy to come up with
replacing `#blah` with the dynamic definition of a method.

And that was it. A whirlwind introduction to metaprogramming and ruby macros in
a nutshell. Just to ensure understanding, I asked Braden to rewrite another
rails method, like `validates` just to ensure that he understood the concept of
macros.

Later in the afternoon, we ran across an example of a macro, and he was easily
able to understand what was actually going on. This was probably one of my
proudest moments at CoverMyMeds. Less than 72 hours from my last day, and I felt
a strong sense of pride and hope for my team. 

To summarize, Mentor-Mentee pairing starts as Driver-Navigator, where the mentee
is an active navigator or active driver. And when a concept comes up, that's
confusing, the mentor steps in and discusses the concept, and walks the mentee
through examples to ensure understanding. Finally, the pair returns to their
work. Very much a teach a man to fish scenario.
