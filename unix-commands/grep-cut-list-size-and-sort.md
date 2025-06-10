# grep, cut, list size, and sort

Grep a keyword in a directory, cut some output, and list file size and then sort unique

> grep a keyword directory | cut -d ":" -f 1 | xargs ls -l | sort -u

