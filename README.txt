mkvrechapter is a script that uses mkvmerge to fix chapter indices for
MKV audio/video files that have been split by mkvmerge.

Example: You have a file tentacles.mkv that has 10 chapters in it, but
the content is a concatenation of episodes from your favorite hentai
anime series and you want to split it into

* Episode 1 is on chapters 1-3
* Episode 2 is on chapters 4-5
* Episode 3 is on chapters 6-10

You split this file into episodes like this:

mkvmerge -o tentacles-e.mkv --split chapters:4,6 tentacles.mkv

so that you now have these episodes in your current directory:

* tentacles-e01.mkv
* tentacles-e02.mkv
* tentacles-e03.mkv

Trouble is, these files now have weird chapter indices.  E.g., 

* tentacles-e01.mkv Chapters=1,2,3
* tentacles-e02.mkv Chapters=4,5
* tentacles-e03.mkv Chapters=6,7,8,9,10

This is more embarrassing than someone finding these videos on your
hard drive.  To fix it, run mkvrechapter like this in the shell:

for i in tentacles-e??.mkv; do
    mkvrechapter ${i} fixed-${i}
done

Now the chapters will be

* tentacles-e01.mkv Chapters=1,2,3
* tentacles-e02.mkv Chapters=1,2
* tentacles-e03.mkv Chapters=1,2,3,4,5

Note that mkvrechapter uses mktemp, so temporary files will be placed

1. Under $TMPDIR if it's set
2. Under /tmp if it's not set

So, if you'd rather make temporary files on a hard drive that can
handle getting mercilessly pounded by massive videos, do something
like

TMPDIR='.' mkvrechapter ... # tmp files go in current directory

mkvrechapter is in the public domain.
