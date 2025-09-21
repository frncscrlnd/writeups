---
layout: default
title: Level 12 → Level 13
order: 13
---

# Level Level 12 → Level 13
After logging in with 

`ssh bandit12@bandit.labs.overthewire.org -p 2220`

Password: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

and running `ls` we can see that there is a data.txt file inside the /home/bandit11 directory. As the game tells us, the file is a [hexdump](https://en.wikipedia.org/wiki/Hex_dump) of a file that's been repeatedly compressed. The game also tells us to use temporary directories or choose hard to guess directory names. We'll go with the first option: lets `cd` to `/tmp` (`cd /tmp`) and make a temporary directory with `mktemp -d`, which will return the path to the directory (e.g `/tmp/tmp.nNrWI3pdyH`). 

We'll then copy the content of `data.txt` to this directory with `cp ~/data.txt .` (`~` stands for the `home` directory and `.` is the directory you want to copy the file to). We can also rename it with `mv data.txt hexdata.txt` (`mv` stands for move; the second field is the new file name). 

I renamed it `hexdata` since our file is still a hexdump. We now need to convert it back to compressed data. We can do this by using `xxd`: this command normally returns the hexdump of a file, but by using the `-r` flag we can revert it. Here's how:

`xxd -r hexdata.txt zipdata` (where zipdata is the name of the new file).

We can now move on to decompressing the file. Before decompressing we need to know which decompression we need, and we can do this by looking at the hexdump: `xxd hexdata` will give us the hexdump, which starts with `1F 8B`. We can compare this with a [list of all file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures) adn finally find out that this is `gzip`'s file signature. So, first, we have to rename our zipdata file to zipdata.gz with `mv zipdata zipdata.gzV (`gz` is gzip's file extension). Then. we'll use the compressing program gzip; with this program you can compress a file with `gzip file.txt` and decompress it with `gzip -d file.txt.gz` or `gunzip file.txt.gz`. In our case we'll have to use:

`gzip -d zipdata`

This will return another zip folder, as we can see with `xxd zipdata`, which returns a hexdump that starts with `42 5A 68`, bzip2's file signature. This means we'll have to rename the file to fit bzip2's extension: `mv zipdata zipdata.bz2`. We can then run:

`bzip2 -d zipdata.bz2`

just like we did with gzip. This will return another zip file (run `xxd zipdata` again) and this time it starts again with gzip's file signature. Let's rename it with `mv zipdata zipdata.gz` and run:

`gzip -d zipdata`

again and finally we get a... tar archive, as we can see with `xxd zipdata` and seeing that the filename data5.bin appears. this means that `zipdata` is a tar archive. 

Let's rename our file to a tar archive with `mv zipdata zipdata.tar` and extract it with 

`tar -xf zipdata.tar`

Will return the `data5.bin` file we saw in the archive. Running xxd again prints out the name of another file (data6.bin), so we're looking at a tar archive again.  Let' extract it with

`tar -xf data5.bin`

Running `xxd` on the resulting `data6.bin` file returns bzip2's file signature again; extract it with:

`bzip2 -d data6.bin`

Running `xxd` on the resulting `data6.bin.out` file allows us to find out that it's another tar archive with a `data8.bin` file inside. Let's get its' content with 

`tar -xf data6.bin.out`

`xxd` on the resulting `data8.bin` returns gzip's file signature again. Let' rename our file with `mv data8.bin data8.gz`, then decompress it with:

`gzip -d file8.gz`

Running `cat data8` returns our password:

`FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`



