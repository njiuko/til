# List and sort directory size

`du -cks / | sort -nr`

Prints all files and subfolders of the current directory ordered by size to the current shell.
Usefull in order to identify which folder/file takes the most space in your fs.
