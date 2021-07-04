# Typeahead-System
A typhead system for Movie Recommendations
### Completion in lexicographical ordering
This can be achieved by using the sorted set feature of Redis in O(log(N)) time. In Addition to arranging the items in a sorted maner they are also indexed(0 - based indexing) and the rank can be obtained using the <b>ZRANK</b> Command. <br>
![r2](https://user-images.githubusercontent.com/69399113/124376516-f52d0080-dcc4-11eb-8097-e1dcb60ec41c.JPG)
<br>
Now to obtain the search corresponding to the prefix we need to store all the prefix of the the given word.So we also add "bar*". If I add the word "foo", "bar", and "foobar" using the above rules, this is what I obtain:<br>
![r3](https://user-images.githubusercontent.com/69399113/124376618-869c7280-dcc5-11eb-8b4b-453cd3297720.JPG)

If the user wants to have the list of all suggestion with the word <b>fo</b> , the result would be be 
1. "foo"<br>
2. "foo*"<br>
3. "foob"<br>
4. "fooba"<br>
5. "foobar"<br>
6. "foobar*"<br>
The time complexity to retrive the suggestions would br O(log(N)) byt the memomry requirements would be huge . If M_avg is the average number of prefixes the memory requirement would be O(N*M_avg)<br>
### Optimisations on space
1. we can optimise space by <b>inserting prefixes of atleat length 3 </b>which means the user will have to type atleast 3 letters to retrieve suggestions<br>
2. <b>Don’t store prefix starting with stop words</b> (if that stopword is not the first word of the movie name)<br>
This observation was based on the historical data of the searches we had. People generally don’t start their search with a stop word like (a, an, the, of etc) unless it is the first word of the name. For example, the possible search attempts for The Pursuit of Happyness could be “the pursuit”, “pursuit of ”, “happyness” but not “of happyn”<br>
3. <b>Limiting prefix max length to 8 char</b><br>
For example, after restricting the maximum length of prefix to 8 for “The Lord of the Rings”, the keyspace is drastically reduced and the average cardinality will be more than 1 as same prefix will be shared by many movie names
