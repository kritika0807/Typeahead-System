# Typeahead-System
### Completion in lexicographical ordering
This can be achieved by using the sorted set feature of Redis in O(log(N)) time. In Addition to arranging the items in a sorted maner they are also indexed(0 - based indexing) and the rank can be obtained using the <b>ZRANK</b> Command. 

