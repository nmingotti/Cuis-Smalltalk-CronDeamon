# Cuis-Smalltalk-CronDaemon

* Provides something similar to Unix **cron** daemon to run inside Cuis Smalltalk.
* It makes possible to run a **block** every time a time Duration elapses. For example: run something every 2 hours.

## How does it work

* When instantiated **CronDaemon**  runs a background process, this process checks if any **CronUnit** subclass
  instance is ready to be run. If so, it runs it.
* There are two subclasses of **CrontUnit**, namely **CronUnitEvery** (which runs a process every X time amount) and
  **CronUnitAt** which runs at specific times&dates (this at the moment is not implemented).
* You can make only one instance of **CronDaemon** which is accessible via `CronDaemon default`.
* `CronDaemon default` can be *enabled* or *disabled*, by default it is created in **disabled** state. 
* **CronUnit** instances can be *enabled* or *disabled*, by default they are created in **enabled** state.
* Differently from Unix *cron*, *CronDaemon* isn't stateless. It remembers last time a CronUnit has been run, for example.
* **CronDaemon** does not store in itself the instances of **CrontUnit**, you are supposed to store them
  into your own code instance variables. **Example**, suppose you have a class *FooClass* which would like to run 
  a method *fooMeth* every 1 hour. What you need to do is define an instance variable *fooTimer* in your class *FooClass*
  and store in the *fooTimer* the instance of a subnclass of **CrontUnit**.

## Examples 

* These examples are a copy of what is available in the **CronDaemon>>README**. 
```smalltalk 
		
". Create the CronDaemon which wakes up every 2 seconds to check what units has to be run
. NOTE. 2 seconds is a very short time, it is just an example for test, you may want to put a longer time period.
"
CronDaemon new: (Duration seconds: 2).
" . the CronDaemon is created in disabled state, to make it work you must enable it. "
CronDaemon default enable. 
" . You may want to see when the CronDaemon wakes up and check who has to be run."
CronDaemon default transcriptLogQ: true. 	
		
				
cu1 _ CronUnitEvery new: (Duration seconds: 3) name: 'test unit 1' 
				    do: [ Transcript log: 'running unit 1' ] .
		
cu2 _ CronUnitEvery new: (Duration seconds: 12) name: 'test unit 2' 
				 do: [ Transcript log: 'running unit 2' ] .

CronDaemon default listAllUnits . 

". disable / enable a CronUnit"
cu1 disable. 
cu1 enable.

". if you delete a unit it is gone, CronDaemon does not store them. "
cu1 _ nil.       ". cu1 is gone, CronDaemon will not find it anymore, unless it has some other reference somewhere. "

". disable / enable the CronDaemon "
CronDaemon default disable. 
CronDaemon default enable. 

". disable/enable logging daemon runs in Transcript " 
CronDaemon default transcriptLogQ: false. 
CronDaemon default transcriptLogQ: true. 
```



## TODO 
* Run something at specifics time and/or date, that is, implement **CronUnitAt**.


