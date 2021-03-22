---
date: "2020-11-07"
tags: [""]
title: "some pointers in taste"
toc: false
---

In 2016 Linus Torvalds, who created Linux (the OS {wording}) and `git` (the version control system used to manage this blog), sat down for with Chris Anderson, who is in charge of Ted, for a chat. [The interview](https://www.youtube.com/watch?v=o8NPllzkFhE) is about 22 minutes long. If you have the time, it's well worth watching. 

During the interview, Chris asks Linas about his concept of "taste". Linas had brought two code snippets, one that was bad "taste" and one that was good "taste". He explained the difference between the two,

> Sometimes you can see a problem in a different way and re-write it so that a special case goes away and becomes the normal case. And that's good code

For me, this is the shared essence of computer science and mathematics. There is an unparalleled joy in understanding a beautiful idea. By the end of this blog, you'll understand it, and I hope you derive joy from it too (:

---

* Two code blocks side by side

The bad taste version.

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
The good taste version
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

Now to explain the idea

Reader is going to need 

* A way to think about "memory"
* An understanding of linked lists, a way to store complex data within this memory structure
* The first method 
* The second method
* The elegant differences!



### memory 

Before we can appreciate the differences between these approaches (and also what's going on), we need a working understanding of computer memory. Consider a row of school lockers, laid out in a line from left to right. 



You can put only one number or letter in each locker[^1]. We'll label the value you put in the locker, drawn in red, as the data. 



[^1]: To be precise, each "unit" of memory is one byte wide. I've avoided that for clarity's sake

### linked lists

We often want to create more interesting ways of storing data than this dull row of blocks. But that is what we have to work with. If we want to do something else, we must overlay it on top of what we have. One of those more interesting structures is a _linked list_. Here's the idea,

### first method

This is pretty easy to grasp. Go through element by element, until we get there



### second method

Get the pointer to the first entry, not just the first entry. Then walk the list

