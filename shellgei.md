- for i in `seq 1 1 8`; do wget "http://example.com/$i.pdf"; done;
- ps aux | egrep '*virt*' | grep -v 'grep' | xargs -L1 | cut -d ' ' -f 2 | xargs kill -9


