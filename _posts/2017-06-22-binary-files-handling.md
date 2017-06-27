---
layout: post
title: "Handling binary files in C"
status: released
type: post
comments: true
list: etut
set: jipp
---

Handling binary files isn't as easy as text files. You have to know exact format prior to attempting handling them, but they come with one big advantage: they are much faster to read.

# When and why they are used

They are often used when format won't change often or it won't change at all. Some of game developers while packaging their games intentionally bundle their resources and ship them in big boxes. Actually performance gain here is very big. Instead of reading hundreds of files you read one or two of them. Also keep in mind that if you are storing data in text format (such as JSON or XML) you will have to convert numbers to according formats which takes precious time and computing power.

Binary files are not pure gold though. They come with few drawbacks. If you are not careful while writing you might not always be able to read the file. For example files in 64 bit system you might not be able to read it on 32 bit due to variable types sizes differences.

# How are we going to tackle it?

Handling binary files isn't much different than it's text brothers. As usual, first thing you have to do is open the file.

{% highlight c %}
  FILE* pf = NULL;
  fopen_s(&pf, filename, "mb");
{% endhighlight c %}

M stands here for mode. If you want to write you'll have to put there `w` and when reading `r`. Please read through documentation about it.

After this first very easy step you can perform `fread` and `fwrite` operations. But just writing random stuff into the file might not always be good enough. If you have complex structures it might be troublesome. Data written sequentially you will have to read sequentially.

# Let's add a little bit of architecture there!

With some design in mind we can eliminate that sequential read. What if we wrote somehow offsets to each of the elements we want to store? We technically could do that! All we would have to do is to store number of elements first.

For that to work you need to learn about another few helper functions. Mainly about `ftell/1` and `fseek/3`.

First one is pretty straight forward. `ftell(File*)` will return offset from the begging of the file till current point you are at.

`fseek(File*,long int, int)` is general opposite of said function. It will take a file, offset and modifier and it will move pointer to position designated by modifier and offset. There are three main modifiers you can apply.

| Modifier | Action |
| -------- | ------ |
| SEEK_SET | From the beginning of the file |
| SEEK_CUR | From the current position of file's pointer |
| SEEK_END | From the end |

What have may guessed we are going to `ftell` our position before writing, save it to some collection and after we write to file we are going to write down that collection to file.

# Become TAXI MASTA

Let's assume we want to make a Cab manager. Not fancy one with cute and clear web interface and highly scalable backend. Let's assume it's more like prehistoric era and maybe Drivers have devices that can record each ride and write them at the end of the day on some kind of hard drive or something. First of all we are gonna do some modeling. Each ride should have an address of start and the end location, time spent on serving customer and maybe distance travelled.

{% highlight c %}
struct TaxiRide{
  char* start;
  char* end;
  double minutes_spent;
  double distance_travelled;
};
{% endhighlight %}

Now that we have our data model we can finally start writing it! Let's start by preparing some variables and opening the file.

{% highlight c %}
size_t it, no_items = your_data_structure_number_of_elemnts;
unsigned int no_it = (unsigned int)no_items;

__int64* offsets = (__int64*)malloc((no_items + 1) * sizeof(__int64));
if (!offsets) // Checking whether memory has been allocated
  handle_error(MEMORY_ALLOCATION_ERROR);

FILE* pf = NULL;
fopen_s(&pf, filename, "wb"); // w - for write, b - for binary

if (!pf) //checking whether file has opened
  handle_error(FILE_PROCESSING_ERROR);
if(fwrite(&no_it, sizeof(unsigned int), 1, pf) != 1)
  handle_error(FILE_PROCESSING_ERROR);

//We are moving forward for the amount of memory we allocated for later writer.
_fseeki64(pf, (no_items + 1) * sizeof(__int64), SEEK_CUR);

{% endhighlight %}

The most interesting thing in this code is type choice. I chose `__int64` mainly because it's big and it has constant size over both 64 and 32 bit platforms. For same reason I didn't use `size_t`. `size_t` is a macro that returns either `unsigned int` or `unsigned __int64`. (Check for yourself, right click the type and hit `Go to declaration`) As you can see I used those types either way, but I've done it explicitly. If you would use that macro here files you write wouldn't be cross platform. Written on 64 bit couldn't be read on 32 bit due too variable type size differences. (Program would want to read __int64 where int is written etc.)

Writing is actual data is much easier. I like to start by creating support structure that will hold lengths of strings to store. We need to do that to ease reading later.

{% highlight c %}
struct TaxiWriteHelper {
	unsigned int start_length;
  unsigned int end_length;
};
{% endhighlight %}

Once we've done that we can proceed with writing all the structures in our collection.

{% highlight c%}
for (it = 0; it < no_items; ++it)
	{
    //Elem is nth element of your collection
		file_desc[it] = ftell(pf);
    TaxiRide* ride = (TaxiRide*)elem;
    TaxiWriteHelper helper = { (unsigned int)(strlen(ride->start) + 1), (unsigned int)(strlen(ride->end) + 1) };
    fwrite(&helper, sizeof(TaxiWriteHelper), 1, file);
    fwrite(ride, sizeof(TaxiRide), 1, file);
    fwrite(ride->surname, sizeof(char), helper.start_length, file);
    fwrite(ride->surname, sizeof(char), helper.end_length, file);
	}

	file_desc[it] = ftell(pf);

	_fseeki64(pf, sizeof(unsigned int), SEEK_SET);
	fwrite(file_desc, sizeof(__int64), no_items + 1, pf);
{% endhighlight %}

Now you need to just close the file and voila! You've done it!

# Now the other way...

Writing things to files isn't useful at all when you can't read what you've written. But really, it is really straight forward. You have to do everything you have already done, but in reverse! Instead of using `fwrite` you have to use `fread`. It's as simple as that! Mostly...

{% highlight c %}
    fread(&no_items, sizeof(unsigned int), 1, pf);
	file_desc = (__int64 *)malloc((no_items + 1) * sizeof(__int64));
	if(!file_desc)
		handle_error(MEMORY_ALLOCATION_ERROR);
	fread(file_desc, sizeof(file_desc[0]), no_items + 1, pf);
	for (it = 0; it < no_items; ++it)
	{
		_fseeki64(pf, file_desc[it], SEEK_SET);
		TaxiRide* ride = NULL;
        TaxiWriteHelper* helper = NULL;
        char buffer[512];

        fread(helper, sizeof(TaxiRideHelper), 1, pf);
        fread(ride, sizeof(TaxiRide), 1, pf);
        ride->start = NULL;
        ride->end = NULL;
        fread(buffer, sizeof(char), (size_t)writeHelper.start_length, pf);
        //Allocate and copy buffer to start
        fread(buffer, sizeof(char), (size_t)writeHelper.end_length, pf);
        //Allocate and copy buffer to end
	}
{% endhighlight %}

# Conclusion

Here you go! It is a little brain stretch with all those things you have to keep in mind. I intentionally left some code for you to fill. Feel free to implement this exercise and ask questions if you have any.
