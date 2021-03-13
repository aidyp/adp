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

In 2016 Linus Torvalds, who created Linux (the OS most of the internet runs on) and `git` (the content management system this blog uses), sat down for a chat with Chris Anderson, who is in charge of Ted (the place where smart people show off), for a chat. [The full interview](https://www.youtube.com/watch?v=o8NPllzkFhE) is about 22 minutes long. If you have the time, it's well worth watching.

During the interview, Chris asks Linus about his concept of "taste" (@ 14:20, video will automatically start from here). Linus had brought two code snippets, one that was bad "taste" and one that was good "taste". He explained the difference between the two,

> Sometimes you can see a problem in a different way and re-write it so that a special case goes away and becomes the normal case. And that's good code

For me, this is the special sauce. Whatever reasons one is drawn to computer science, this is usually the reason they stay. By the end of this blog, you'll have the palette to appreciate why the second piece of code is in better taste, and I hope you'll derive some pleasure from it like the rest of us nerds.

---


### memory 

Before we can appreciate the differences between these approaches (and also what's going on), we need a working understanding of computer memory. Here's a brief introduction.

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

Then all we need to keep track of is the address of the first element. Given this first address, usually styled as the `head`, we can go forward in the list from there.

In (C) code this idea is described like so,

```
struct ListElement
{
	char value;
	ListElement* next_element;
};
```

And then we have a special element, `head`, that marks the start of the list

```
ListElement* head = 
```

Because every element is linked to the next one, we need only keep track of the `head`. To get to any subsequent element, we simply start from the `head` and follow the links until we reach the desired element.

{picture here}



---

### first method

We are now (at last!) equipped to understand both methods, and the 'aesthetic' qualities of the two. Both of them are concerned with the same task: how, given a list with some entries in it, can we remove an entry?

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

Let's take a moment to re-examine the shape of our linked list. We have some blocks of memory connected to each other. At the beginning, we have the `head`. The `head` points to the first element of the list. That pointer must be stored _somewhere_ in memory. For the sake of pedagogy, let's say it's stored at `-1` 

![Alt](/pictures/taste/linked_list_head.png#center)

Now suppose we get the pointer to that one

![Alt](/pictures/taste/linked_list_indirect.png#center)



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