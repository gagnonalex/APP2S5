# APP2 S5

### RTOS communication objects
* `Queue` : A list containing pointers to certain objets. Is automaticly allocated on creation depending of the object type. If the data in the list needs different treatment depending a specific data, it should be linked to a `Memory pool` or a `Mailbox` could be used.
* `Memory Pool` :  Preallocating a number of memory blocks with the same size. The application can allocate, access, and free blocks represented by handles at run time.
* `Mailbox` :  A mix of two element previously develed : the mailbox conatina a pointer to the object instanciated AND can contains data. Maker sure to have one MailBox per type of data 

