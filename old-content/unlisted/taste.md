---
date: "2021-01-31"
tags: ["programming"]
title: "some pointers in taste"
toc: false
---

This is more technical than other posts of mine. 

---

### tux at ted

{{< youtube id="o8NPllzkFhE?start=860" >}}

In 2016 Linus Torvalds, who created Linux (the OS most of the internet runs on), sat down for a chat with Chris Anderson, who is in charge of Ted (the place where smart people show off), for a chat. [The full interview](https://www.youtube.com/watch?v=o8NPllzkFhE) is about 22 minutes long. If you have the time, you should watch the whole thing.

During the interview, Chris asks Linus about his concept of "taste" (@ 14:20, video will automatically start from here). Linus had brought two code snippets, one that was bad "taste" and one that was good "taste". He explained the difference between the two,

> Sometimes you can see a problem in a different way and re-write it so that a special case goes away and becomes the normal case. And that's good code

This is the special sauce. Whatever reasons one is drawn to computer science, this is usually the reason they stay. By the end of this blog, you'll have the palette to appreciate why the second piece of code is in better taste, and I hope you feel the rush.

---


### memory 

The code snippets that Linus brought concern **linked lists**, which is a way of managing computer memory. If you're already familiar with this concept, you can skip ahead to the [first method](#first-method)

Imagine a row of lockers -- like the ones you had at school -- laid out in a line.

![Alt](/pictures/memory_example.png#center)

Each locker contains one letter or number. We'll label that data red. Each locker also has a unique locker number, so we can tell one locker from another. We'll label that -- the reference -- blue. 

Let's examine the first piece of data stored, "h". There are two ways refer to it. The first is _directly_. We can  talk explicitly about the "letter h". The second way is _indirectly_. We can talk about "the letter in locker 0". They both refer to the same thing, but in two different ways. 

This second notion, of reference, is captured in programming languages by **pointers**. They point to the _location_ in memory where data is stored, rather than the data itself.

In C, this different way of addressing things looks like this,

```
/* C-ing clearly! */

/* Keeping track of something directly */ 
char direct = "a";

/* Keeping track of something indirectly with a pointer */
char* indirect = &direct;
```

For those unfamiliar with C this can look like, well, a different language. Let's translate.

* `char` corresponds to the _type_ of data being stored. It's short for "character". Different data types are classified differently. For example, `int` for an integer. Keeping track of data types makes sure we don't accidentally use a number when we really need a letter.
* `char*` corresponds to a _pointer_ to `char`-type data. 
* `&` is the symbol C uses when you want to get the pointer to some data, rather than the data itself. In this case `&direct` is the _address_ of `direct`. It's the number of the locker that holds that data. 

![Alt](/pictures/direct_indirect.png#center)



### linked lists

It works well enough to store things in this way. But sometimes we want more intricate ways to store and manipulate data. To satisfy this goal, we need a new idea. And because the above memory model is what we have to work with, we must layer that new idea on top of the model.

One of those more detailed structures is a _linked list_. The idea is we want to store some data as a collection of elements, where each element links to the next one. Here's a picture

![Alt](/pictures/linked_list.png#center)

We can implement this by storing both the element in the list, and the link to the next element[^1], in memory. 

[^1]: `NULL` is a special pointer in C. It tells the computer that there's nothing to point to. It's used to mark the end of the linked list. The final element points to `NULL`, which the computer knows to mean that there's no more elements.

![Alt](/pictures/linked_list_memory.png#center)

This is especially neat because the elements don't have to be close to each other. As long as each element points to the next one the properties of the list are preserved, no matter how far away they may be from each other.

![Alt](/pictures/taste/linked_list_mem_long.png#center)

Then all we need to keep track of is the first element. Once we have this first element, usually called the `head`, we can go forward in the list from there.

In (C) code this idea is described like so,

```
struct ListElement
{
	char value;
	ListElement* next_element;
};
```

And then we have a special element, `head`, that marks the start of the list.

```c
ListElement * head = NULL;
/* We reserve some memory for the element */
head = (ListElement *)malloc(sizeof(ListElement))

/* Then we can fill in the memory we have reserved */
head->value = 'h';
head->next_element = NULL; // For now there are no other elements
```



Because every element is linked to the next one, we need only keep track of the `head`. To get to any subsequent element, we simply start from the `head` and follow the links until we reach the desired element.

{picture here}



---

### first method

Both methods are concerned with manipulating a linked list. Specifically, how can we remove an element from a linked list?

The first method, the one in "bad" taste, is the natural way we might go about it. 

Suppose we have the example linked list from earlier and we want to delete the second entry (the one containing the letter 'e').

![Alt](/pictures/delete_e_linked_list.png#center)

We start at the `head` of the list. Then we walk the list until we reach the entry we are trying to delete. 

![Alt](/pictures/delete_e_linked_list_2.png#center)

Voila. The entry is no longer in the list. It has, for all intents and purposes, been deleted.[^2]


[^2]: This is not strictly true. There must be some care taken to release the memory -- to allow those lockers to be used for some other purpose -- which is done outside of this.

This method works well, with one small but sharp exception. What if we want to remove the _first_ entry from the linked list?

We start at the `head` of the list as usual. 



{{< expandable label="Code" level="3">}}

```
remove_list_entry(entry)
{
	prev = NULL;
	walk = head;
	
	// Walk the list
	while (walk != entry)
	{
		prev = walk;
		walk = walk->next;
	}
	
	// Remove the entry by updating the head
	// or previous entry
	
	if (!prev)
		head = entry->next;
	else
		prev->next = entry->next;
}

```
{{< /expandable>}}

---

### aesthetics

It's a good question. What _is_ ugly about the first method? It's practical. It's easy to follow. And, it works.

It has **edge cases**. The same solution does not apply everywhere. It must pay special consideration to one area of the problem, and treat it differently.

Imagine you are wrapping a present for someone. You cut the wrapping paper into a shape and size that will cover the gift. You fold the paper over the gift, and it is covered smoothly. Until you realise that there is this bit in the corner that doesn't line up properly. So you get the tape out and stick down the offending edges. It works, the present is wrapped. But it's ugly. There's a bodge job in the corner that you've fixed with a tangle of tape and crumpled wrapping paper. 

![Alt](/pictures/taste/bad_wrapping.png#center)

Sure, when you give the present, you can hand it over ugly-side down and it'll work fine. But you'll always know it could have been better.



It _can_ be better. If you take some time to think about the shape of the biscuit tin, you can cut the wrapping paper in a different way. Then, you can wrap it smoothly, without any ugly edges. A single solution covers the whole problem.

![Alt](/pictures/taste/good_wrapping.png#center)

---

### second method

We're going to re-examine the shape of a linked list.

#### shape of a linked list

Let's start with the shape of a linked list. A linked list is made of elements. Each element has two very important properties,

* That elements *points* to something.
* That element is *pointed to* by something.

In the case of elements in the middle of the list, these two 'somethings' are clear: they are other elements. An element in the middle of the list points to the next one, and is pointed to by the previous one.

But what about the elements at the two ends of the list? The final element, let us call it the `tail`, points to `NULL` (to indicate the list is over). 

The first element, points to the second element. But what is it pointed to by? 

The variable, `head`. `head` is a pointer to the first element of the list. And that pointer is stored _somewhere_ in memory.

![Alt](/pictures/taste/linked_list_head.png#center)

For simplicity, we will say `? = -1`, and that is the location in memory of the `head`, the pointer to the first element in the list.

{diagram like above with the reference of `-1}

This gives us a new way to think about how a linked list is structured. Each element has a pointer, somewhere in memory, that points to that element. We can list them out,
1. The pointer at address `-1` points to the first element of the list. 
2. The pointer at address `1` points to the second element of the list.
3. The pointer at address `4` points to the third element of the list.
4. The pointer at address `7` points to `NULL`, which terminates the the list.

{diagram with all the pointers referenced}

For a linked list, 'deleting' an element is equivalent to breaking the links for that element, and re-linking the element before and after. In method one, the strategy was to walk element-by-element to find the target for deletion. In method two, the strategy is to go _pointer-by-pointer_. To re-link an element, simply re-direct the previous pointer. Suppose we want to remove the second element of the list. We know that the pointer at address `1` points to the second element.

{Diagram highlighting the pointer to the second element}

Then, as before, we redirect that pointer to the third element. More precisely, we edit the pointer held in address `1`.

{Diagram editing the pointer}

This method works for the first element too. To delete the first element, simply edit the pointer at address `-1`.

{Diagram doing the same for the first element}

And that's it! One solution, that covers the whole problem.



{{< expandable label="Code" level="3">}}

```
remove_list_entry(entry)
{
	// The "indirect" pointer points to the
	// *address* of the thing we'll update
	
	indirect = &head;
	
	// Walk the list, looking for the thing that 
	// points to the entry we want to remove
	
	while ((*indirect)) != entry)
		indirect = &(*indirect)->next;
		
	*indirect = entry->next;
}
```
{{< /expandable>}}

---

### exit(0)

This is a small solution to a small problem. 

