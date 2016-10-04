# cornerstone
A Raft Consensus C++ implementation, the original implementation was published by [Andy Chen] (https://github.com/andy-yx-chen), as he agrees,  we re-organize his source code, and republish under the same license.

To respect Andy Chen's work, we keep using **cornerstone** as the project's name and we will start iterating based on his work.

## Features
- [x] Core algorithm, implemented based on TLA+ spec (though the spec does not have timer module)
- [x] Configuration change support, add or remove servers without any limitation
- [x] Log compaction
- [x] **Urgent commit**, enables the leader to ask all other peers to commit one or more logs if commit index is advanced
- [x] Client request support, for each server, the state machine could get a raft consensus client to send request to the leader, so leader change listener is not required.

## Where to start?

The key advantage or could be disadvantage for this implementation is it does not have any additional code that is unrelated to raft consensus itself. Which means, it does not have state machine, which could be a storange service.
It also has very few dependencies, actually, only the STL and asio. It chooese asio is because asio supports both timer framework and async socket framework, which could be run on Windows, Linux and BSD systems.
The project only contains the following stuff,
- 1. core algorithm, raft_server.cxx
- 2. fstream based log storage
- 3. asio based timer implementation
- 4. asio based rpc server and client (through tcp)
- 5. buffered logger implementation

You are not able to get an exe file to do something meaningful by building the project, actually, you will get archive file instead (or lib file on Windows), however, you can build the test project and see how to use it and how it would work

You most likely should start with test_impls.cxx, under test/src folder, as that contains a use case of 
- what need to be implemented to leverage the core algorithm
- how to use the core algorithm
- how to send logs to cluster 

## Build and run

### On Windows

Please use nmake to do the job

The following command will build the test project and run all tests
> nmake -f Makefile.win tests

The following command will build the lib file,
> nmake -f Makefile.win all

### On Linux

Please use gc make to do the job

The following command will build the test project and run all tests
> make -f Makefile.lx test

The following command will build the lib file,
> make -f Makefile.lx all

### On FreeBSD

Please use pmake to do the job

The following command will build the test project and run all tests
> make -f Makefile.bsd test

The following command will build the lib file,
> make -f Makefile.bsd all

## A popular question

Why ptr\<T\>?

Actually, it's the same as shared\_ptr with make\_shared, however, it's syntax is simpler, such as concepts of ptr\<T\> and ptr\<T&\> instead of shared\_ptr and weak\_ptr and also, after you private the constructor and make it friend to cs\_new, you dont need enable\_this\_from\_shared, you can simply use cs\_safe to covert _this_ pointer to a ptr\<T\> object.

## Contact Us

For any questions, please contact [Us](mailto:github@data-technology.net)