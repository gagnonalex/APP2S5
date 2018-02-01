# APP2 S5

## RTOS vs GPOS
**GPOS** : general purpose os   
* More use in a pc kind of machine, huge amount of processing power and memory, still have access to real time SW


**RTOS**: real time OS 
* Consume less memory, is compact and scalabale
* you can always predict the event going on since it's a simple task every x ms ( lpc 1768 is 5 ms)
* **GPOS** is less stable since its less for efficency since it's not only one task
* **RTOS** is more portable and more reliable

Also in RTOS, we have :
* **Hard real time** : need to be well programmed and timing is key or else it doesn't work 
    
        example: a serial port needs to activate a airbag ... if the reaction time is delayed by 3 second the driver can easily die

* **Soft real time** : doesnt really need to be perfectly timed, if not quality of execution decrease
        
        example : A delay in the writting of data on a serial port for a school project 

**Both** are using multi-threading ans use OS services such as :

 *  **Time**:
   
     * Ticker
     * Interrupt
* **Memory**
    * Is managed by the system itself ( more **GPOS** because their is a LOT of thread)
* **Periphericals**
    * more **GPOS** with, for example a printer that will be shared between mutiple process
* **Communicaton** 
    * Semaphore
    * Mutex
    * ... see [RTOS OBJECT](#rtosobject) 
## RTOSObject
* Communication
    * Mailbox
    * Queue
    * Memery Pool
* Synchronisation
    * Signal
    * Mutex
    * Semaphore ( binary or count)
* Task/Thread
    * Ticker
    * Thread
    * Timer
    * Interrupt

## Thread

### Thread Priosisation
Here are the step to start an RTOS:
*   Reset Vector
*   Boot Loader
*   Hardware
*   RTOS init
*   RTOS component
*   RTOS start

### Wait
Wait normal = global ( block globally the programm, keep the same priority)
Thread::wait (or os) = Only the thread itself, lower its priority


|                | Thread | ticker                                                                                        | Timer                             | Interrupt                        |
| :------------- | :----: | :-------------------------------------------------------------------------------------------: | :-------------------------------: | :------------------------------: |
| Interrupt      |        | x                                                                                             |                                   |                                  |
| Thread/process | x      |                                                                                               | x                                 | x                                |
| Priorisation   | x      | + than real time                                                                              | default to high but can be change | + than real time                 |
| Limitation     | none   | - Limit of a 30 min ticker<br> - Cannot execute blocking function like mutex malloc semaphore | != ms                             | Cannot execute blocking function |

## Thread "Ordonnance" 

* Round-Robin
    * every taks runs after the other (needs a yield)
    * No Priority
* Premtive Priority
    * Exclusive function on priority ( only one thread per priority)
    * no Yield
    * needs Thread::Wait
* Both
    * Depends on priority
    * When same priority, Round-Robin rules apply

## MBED in general 
When a low priority task is running and a higher priority task needs the ressrouce it is using, the low priority task is cut ( in 5ms time in LPC1768 )

## Thread States
* Ready
* Running
* Block

## Thread Issue to avoid
* **Deadlock** : a situation in computing where two processes are each waiting for the other to finish.
 
        to avoid : use synchronisation tool as semaphore and mutex

* **Starvation** : High Priority task has always something to do, so the low priority task will never be use
* **Priority invertion** : A high priority Thread wait for the answer of a low priority thread that will never execute itself because it's low. 
           
            RTOS has a way of avoiding that : it set the priority of the low thread to a higher state
 
## Synchronisation

| Signal                                           | Mutex                                             | Semaphore                                                           |
| ------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------------------------- |
| Interupt the program independently from the time | Protect ressources from access in critical places | Protect ressources ( not critical since any thread can relaease it) |
| Temporal Synchronisation                         | Link to a Task                                    |                                                                     |
|                                                  | Binary                                            |                                                                     |

 ### RTOS communication objects
* `Queue` : A list containing pointers to certain objets. Is automaticly allocated on creation depending of the object type. If the data in the list needs different treatment depending a specific data, it should be linked to a `Memory pool` or a `Mailbox` could be used.
* `Memory Pool` :  Preallocating a number of memory blocks with the same size. The application can allocate, access, and free blocks represented by handles at run time.
* `Mailbox` :  A mix of two element previously develed : the mailbox conatina a pointer to the object instanciated AND can contains data. Maker sure to have one MailBox per type of data 
