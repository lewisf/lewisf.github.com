---
layout: post
title: "Prolog: What the hell is going on"
date: 2011-12-02 01:55
comments: true
categories: [code, prolog]
---

I understand why prolog is useful, but it's fucking confusing, so 
I'm going to spend some time going through my
thought process for implementing a few prolog functions. Because I figure
it helps to completely think things through:

## Problem 1:
define a functor zip(L1,L2,L3) where L1 and L2 are lists
and L3 is the interleaving of the two lists.
For example: zip([1,2],[a,b],L3) should give
1. X = [1, 2, a, b];
2. X = [1, a, 2, b];
3. X = [1, a, b, 2];
4. X = [1, a, b, 2];
5. X = [a, 1, 2, b];
...

We notice that the head of the List L3 will always be either the first
element of L1, or the first element of L2. So we already know that there
are two cases that we have to handle:

``` prolog
zip([A|As], L2, [C|Cs]) :- A = C. ...
zip(L2, [B|Bs], [C|Cs]) :- B = C. ...
```
so if line 1 isn't true, we test line 2 to see if it's true.
this will handle both cases of the head of L1 being first, or the
head of L2 being first.

but we can simplify, because this is effectively the same as writing

``` prolog
zip([A|As], L2, [A|Cs]) :- ...
zip(L1, [B,Bs], [B|Cs]) :- ...
```

Next we notice that the second element is either the tail of the first list,
or the second list. 

``` prolog
zip([A|As], L2, [A|Cs]) :- zip(As, L2, Cs).
zip(L1, [B,Bs], [B|Cs]) :- zip(L1, Bs, Cs).
```

to finish we notice that we can just throw the rest of the other list into it,
effectively getting a shuffling or interleaving type thing. As zip is continually
called, the input lists we check will get smaller and smaller. So we need a 
base case. Pretty much, this means things should stop when all the lists
are empty. so to finish it off:

```prolog
zip([], [], []).
zip([A|As], L2, [A|Cs]) :- zip(As, L2, Cs).
zip(L1, [B,Bs], [B|Cs]) :- zip(L1, Bs, Cs).
```

## Problem 2: assoc
assoc traditionally a function that when provided with a list of key/value
tuples, and a key, it returns a value or returns nil/false/whatever.
we try to get the same functionality through prolog.

expected behavior is as follows:
``` prolog
?- assoc([[a,1],[b,2],[c,3],[d,4],[b,5]],c,3).
true.
```

so we have a list of size 2 lists ... and we need to iterate
through all of these, and see if the key exists. base case being
if there's an empty list, nothing found, return false, but if 
something is found, return true.

To continue, we do some head tail action and recursively call assoc ...
``` prolog
assoc([[K|[V,_]]|T], A, B) :- assoc(T, A, B)
```
K|[V,_] gets the key and value out of the head of the list of [key,value]
we do the extra [V,_] because a [K|V] would get us K and a [V], as thats how
head|tail works in prolog.

So we have the recursion doing, but we need it to actually stop. There are
two cases when it stops, when a valid key/value pair is found or when the
input list runs dry. the running dry will happen by itself, so we need
to check for a valid key/value pair. to do this, we add a statement to
make this check.
``` prolog
assoc([[K[V|_]]|T], K, V).
assoc([[K|[V,_]]|T], A, B) :- assoc(T, A, B)
```
So now, if K = the key input and V is equal to the value input, we return
true. If not, we recursively call assoc on the next element in the list.
and on and on, if the list runs dry, then it returns false.

## Problem 3: remove\_duplicates

Remove duplicates does exactly what it sounds like, it takes a list
and removes the duplicates. So expected behavior:

``` prolog
?- remove_duplicates([1,2,3,4,2,3],[1,2,3,4]).
true.
```

The way I could tackle this in other languages, is to build another list one
element at a time from the input list, and everytime im adding a value,
i check to see if it's already in the list. In ocaml it would look something
like this:

``` ocaml
let removeDuplicates l = 
  let rec helper (seen,rest) = 
      match rest with 
        [] -> seen
      | h::t -> 
        let seen' = if List.mem h seen then seen else h::seen in
        let rest' = t in 
          helper (seen',rest') 
  in
      List.rev (helper ([],l))
```

don't think this tail recursive implementation will work exactly the same
way in prolog.

Well we do know that what that given:
``` prolog
remove_duplicates([H|T], [H|_].
```

Okay, and we probably need some helper function to check
if something's in a list already ...

``` prolog
check_dupe([value|T], value).
check_dupe([H|T], value) :- check_dupe(T, value).
```

So now we can write
``` prolog
remove_duplicates([], _).
remove_duplicates([H|T], Seen) :- is_dupe(Seen, H) -> remove_duplicates(T, Seen).
remove_duplicates([H|T], Seen) :- append(Seen, H, X), remove_duplicates(T, X).
```

The first line being the base case, where the whole initial list is checked.
The second, checking to see if H is already in the list of seen things, if it is don't
add it to the list of seen things, and just recursively call on tail. If it
hasn't been seen, we move to the third line, where we append it to Seen, and then
call remove duplicates on the tail of the first list and the new seen list.

So this kind of works, unfortunately right now, the behavior is too generous:
it's allowing:

``` prolog
?- remove_duplicates([1,2,3,4,2,3],[1,4,2,3]).
true .
```

but the second list should be in the same order as the first ...

also we're getting some weird mumbo jumbo:

``` prolog
?- remove_duplicates([1,2,3,4,2,3], X).
X = [1, 2, 3, 4|_G363] ;
X = [1, 2, 3, 4] ;
X = [1, 2, 3, 4, _G377] 
...
```
What's wrong? Well, it just seems like its infinite looping right now ...
but my logic is probably wrong. Will be reevaluated tomorrow ...

## Day 2

## remove\_duplicates

remove_duplicates takes two input lists and checks to see that the second
input list is the first input list with duplicates removed.

going to try implementing what i did above in ocaml ...
so we're going to need to imlpement mem, and the helper. mem is pretty much
same as the check dupe above so:
``` prolog
mem(Value, [Value|T]).
mem(Value, [H|T]) :- mem(Value, T).
```


