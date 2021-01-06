# file_read

Reading 16GB File in Seconds, Golang

Once the file is opened, we have below two options to proceed with
1.Read the file line by line, it helps to reduce the strain on memory but will take more time in IO.
2.Read an entire file into memory at once and process the file, which will consume more memory but significantly increase the time.

As we are having file size too high, i.e 16 GB, we can’t load an entire file into memory. But the first option is also not feasible for us, as we want to process the file within seconds.
But guess what, there is a third option. Voila…! Instead on loading entire file into memory we will load the file in chunks, using bufio.NewReader(), available in Go.

The above code introduces two new optimizations:-
The sync.Pool is a powerful pool of instances that can be re-used to reduce the pressure on the garbage collector. We will be resuing the memory allocated to various slices. It helps us to reduce memory consumption and make our work significantly faster.
The Go Routines which helps us to process the buffer chunk concurrently which significantly increases the processing speed.
Now let’s implement the ProcessChunk function, which will process the logs lines, which are of the format

2020-01-31T20:12:38.1234Z, Some Field, Other Field, And so on, Till new line,...\n

We will be extracting logs based on the time stamp provided at the command line.

The above code is benchmarked using 16 GB of a log file.
The time taken to extract the logs is around ~ 25 sec.
