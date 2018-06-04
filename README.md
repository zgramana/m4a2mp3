# m4a2mp3

> Shell script to bulk transcode `.m4a` audio files (e.g. Apple iTunes downloads) to `.mp3` files via VLC. Ideal for parents who want to load purchased iTunes music on kids' music players.

## Usage

Pass in one or more paths to either `.mp4a` files or to directories containing them. When directories are provided, the script will search all sub-directories for `.m4a` files.

Some simple MP3 players don't support special characters like `!` in file names, and those without graphical displays use numeric prefixes on filenames to provide a consistent play order. The script thus outputs a sanitized, and numbered, file name in the destination directory. Output directory defaults to `$PWD/MP3` but can be set by setting the `dst` environment variable, e.g.:

```shell
dst=$HOME/Music/mp3 -n 10 m4a2mp3 /Path/To/Music/Collection/Albums/Kids\'s\ Greatest\,\ Vol.\ 1/
```
results in:
```
/Users/foo/Music/mp3/10_Kids_Greatest__Vol._12_Unicorns__And__Trains_Song__.mp3
/Users/foo/Music/mp3/10_Kids_Greatest__Vol._12_We__Love__Fun.mp3
```

For reasons I don't really understand, or could find much information on, `vlc` on macOS will randomly fail when converting a file via a command line. However, retrying the command will eventally work, though it may take more than one retry.

To make life easier, this script does two things. The first: it generates a new script called `commands.sh` and writes the exact VLC command there. Passing `-x` will automatically execute the script once generated. And two: because it builds a command that prevents VLC from overwriting an existing file, you can simply re-run `commands.sh` until all files have been processed. At that point, delete `commands.sh` otherwise the next time you run `m4a2mp3` the new commands will be appended to the end (and you'll probably see a bunch of pointless error messages).
