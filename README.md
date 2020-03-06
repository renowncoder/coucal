coucal
======

**Coucal**, a Cuckoo-hashing-based hashtable with stash area C library.

![Greater Coucal Centropus sinensis](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Greater_Coucal_%28Centropus_sinensis%29_in_Hyderabad_W_IMG_8962.jpg/250px-Greater_Coucal_%28Centropus_sinensis%29_in_Hyderabad_W_IMG_8962.jpg "Centropus sinensis")

> [Wikipedia] A coucal is one of about 30 species of birds in the cuckoo family. All of them belong in the subfamily Centropodinae and the genus Centropus. Unlike many Old World cuckoos, coucals are not brood parasites.

This is an implementation of the cuckoo hashing algorithm ([paper](https://web.archive.org/web/20180219194838/www.it-c.dk/people/pagh/papers/cuckoo-jour.pdf) by Rasmus Pagh and Flemming Friche Rodler) with a stash area ([paper](https://web.archive.org/web/20160325171418/research.microsoft.com/pubs/73856/stash-full.9-30.pdf) by Adam Kirsch, Michael Mitzenmacher and Udi Wieder), using by default the [`MurmurHash`](https://en.wikipedia.org/wiki/MurmurHash) hash function (by Austin Appleby).

This allows an efficient generic hashtable implementation, with the following features:
* guaranteed constant time (Θ(1)) lookup
* guaranteed constant time (Θ(1)) delete or replace
* average constant time (O(1)) insert
* one large memory chunk for table (and one for the key pool)
* simple enumeration

This library has been thoroughly tested, and is currently used by the [HTTrack](https://www.httrack.com/) project in production.

## License

Copyright © 2013-2014 Xavier Roche (https://www.httrack.com/).
All rights reserved.

This library is licensed under the [BSD 3-Clause License](https://opensource.org/licenses/BSD-3-Clause)
See the [LICENSE](LICENSE) file.

## Example

```c
coucal hashtable = coucal_new(0);
coucal_write_pvoid(hashtable, "foo", "bar");

printf("value==%s\n", (char*) coucal_get_pvoid(hashtable, "foo"));

struct_coucal_enum enumerator = coucal_enum_new(hashtable);
coucal_item *item;
while((item = coucal_enum_next(&enumerator)) != NULL) {
  printf("%s=%s\n", (const char*) item->name, (const char*) item->value.ptr);
}
```
