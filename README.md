# revisit, a TODO list for the future

Many "Getting Things Done"-related systems have a review phase where
you look at all the open tasks and decide whether they are still
relevant.  However, if you work on many projects, a fixed review
interval can be limiting, especially if tasks are spread out across a
more wide timeframe (e.g. quarterly software releases or gardening
projects).

Further, these future tasks often don't have a fixed deadline, so
putting them into a calendar creates clutter and makes regular
rescheduling difficult.

To not forget these future tasks, I wrote a little tool called
`revisit`.  It does a very simple thing: it searches the files passed
for time intervals in a format specified below and shows the intervals
that have passed.  You can then decide to review these tasks, and if
they are meant to be repeated in the future, reschedule them easily
starting from today.  `revisit` will show all lines past their
interval, and also by how many days they are delayed, so you can
easily focus on the tasks that need most attention right now.

## The revisit time interval format

Time intervals are specified in a ISO 8601 based time format.
There are two variants:

	YYYY-MM-DD/P<duration>
	YYYY-MM-DD/+<duration>

The `<duration>` consists of a number and a unit (`Y`ears, `M`onths,
`W`eeks or `D`ays), e.g. `2W` means two weeks and `3M1D` means three
months and a day.

Their semantics as a time interval is the same: the interval is over
after `<duration>` has passed since the given date.  Therefore,

	2021-08-01/P1W is over at 2021-08-08
	2021-08-01/P2M is over at 2021-10-01
    etc

The difference between the `/P` and the `/+` notation is what happens
when we "bump" the interval, i.e. want to update it for the next
review:

* Intervals with `/P` are increased by the duration,
  until the end of the interval is in the future.
  This means an interval using e.g. `1W` will always remain on
  the same week day, even if you bump it on a different weekday.
  Likewise, an interval using `1M` will always remain on the same
  day of month.
  
  Use `/P` intervals if you want to achieve regular reviews
  (at the cost of missing some if you don't review often enough).
  
* Intervals with `/+` are always bumped to today.

  Use `/+` intervals if there is a minimum time that needs to
  pass until reviewing should happen again.

## Gathering timestamps

`revisit` is totally flexible in where you store the time intervals.
You can make a special revisit file and feed it directly to `revisit`,
or spread the tasks out into your projects and their TODO files.

To help search whole file hierarchies for revisit timestamps, the
example script `rg-revisit` is provided, which uses ripgrep for fast
finding.

## Editor integration

`revisit` provides the `-b` flag to bump all intervals when needed.

If you use vim, one way to use this is

	nmap <Leader>b :.!revisit -b<CR>

Then `\b` will bump the timestamp on the current line.

## Copying

Written by Leah Neukirchen <leah@vuxu.org>.
revisit is in the public domain.

To the extent possible under law, the creator of this work has waived
all copyright and related or neighboring rights to this work.
http://creativecommons.org/publicdomain/zero/1.0/
