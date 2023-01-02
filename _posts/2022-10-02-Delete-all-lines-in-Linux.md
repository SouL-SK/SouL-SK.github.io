The `:` introduces a command (and moves the cursor to the bottom).
The `1,$` is an indication of which lines the following command (`d`) should work on. In this case the range from line one to the last line (indicated by `$`, so you don't need to know the number of lines in the document).
The final `d` stands for delete the indicated lines.

There is a shorter form (`:%d`) but I find myself never using it. The `:1,$d` can be more easily "adapted" to e.g. `:4,$-2d` leaving only the first 3 and last 2 lines, deleting the rest.