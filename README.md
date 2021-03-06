# Follow Sync

**Follow Sync** is a little Go program that helps you synchronize your
Instagram "Following" list. Basically, it can unfollow all profiles that are
not following you back.

![Screenshot](https://raw.githubusercontent.com/kirsle/follow-sync/master/screenshot.png)

It uses the Instagram unofficial API ([ahmdrz/goinsta][1]) to log in to your
account (pretending that it's an Android device), compares your followers and
following lists, and provides a summary and a choice of action: go ahead and
unfollow the users who don't follow you back, or exit and you can review the
data on your own.

This program creates a CSV file named `follower-lists-$USERNAME.csv` that
contains your full list, so you can examine what's on your lists or as a backup
in case you want to know who you unfollowed later.

```
$ go install -u github.com/kirsle/follow-sync
$ follow-sync [-wait DELAY=60]
```

By default, this program waits 60 seconds between each unfollow. This is the
safest rate to stay under Instagram's rate limit, but you can override it if
needed. See [Caveats](#caveats) below.

# Program Usage

Just run the `follow-sync` executable. It will prompt for your username and
password (non-echoed output), collect your friend lists and compare them,
and print out a summary.

At this point you can open the `follower-lists-$USERNAME.csv` in your favorite
spreadsheet program if you want to inspect the Following and Follower lists.

The program will prompt for final verification before it goes ahead and begins
unfollowing users.

# Caveats

Occasionally, the Instagram API can get pretty fussy with rate limits. Most
API functions this program calls will panic if it gets an error. No big deal:
you can just wait a few minutes and run the program again and it should pick
up where it left off.

The safest rate limit according to what documentation I could find is to
not unfollow more than 60 profiles per hour. As such, this program has a 60
second delay between each unfollow action by default.

I've seen with shorter delays, such as 2 seconds, that I was able to unfollow
between 600 and 900 users before the API rate limited me.

To change the delay between unfollows, use the `-wait` command line option.
For example:

```
$ follow-sync -wait 2
```

# Distributing

The shell script `dist.sh` can cross-compile the binary for multiple platforms.

Full list of examples for the platforms I export to:

```
./dist.sh linux 64
./dist.sh linux 32

./dist.sh windows 64
./dist.sh windows 32

./dist.sh darwin 64
```

Distributables are put into appropriately-named subdirectories of the `dist/`
folder, and zipped up along with other project metafiles (README, etc.)

# License

```
Follow-Sync: Instagram Follower Synchronization Tool
Copyright (C) 2017 Noah Petherbridge

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA
```

[1]: https://github.com/ahmdrz/goinsta
