# AdvancedAsyncAwait - Tim  Corey
Code and wisdom from : https://www.youtube.com/watch?v=ZTKGRJy5P2M

A simple sample app that downloads data from a list of websites and writes out how long the opperation took. Builds on the AsyncAwait tutorial. Shows how to get **progress** reports back from asynchronous tasks to display a progress bar and more to the screen while they are running. Also sets up the ability to **cancel** a long-running or hung asynchronous task.

![alt text](images\Screenshot20201222.png)

## Parallel.Foreach
RunDownloadParallelSync() is a synchronous method that runs in parallel by using `Parallel.ForEach`. This makes it *as fast* as the parallel async method. BUT UI is still locked.

RunDownloadParallelAsyncV2() is like the sync one but the Parallel.ForEach is wrapped by `Task.Run`, making it async. This will mean that other processes can run while the download is going on.

## Progress Report
Using `IProgress` to get information back from an async task. The following is necessary to do this:

### In Method that calls the async method:
create an `IProgress` to send into the async method and listen for the `ProgressChanged` event on this object:

```C#
//ProgressReportModel is the class (self defined) that contains progress data.
var progress = new Progress<ProgressReportModel>(); 
//On ProgressChanged event, the ReportProgres (self defined) method will be called.
progress.ProgressChanged += ReportProgress;
```

### In the Async Method itself:
Take an `IProgress` as input and call `.Report(ProgressReportModel)` to pass information on to the listeners of the `ProgressChanged` event.

## Cancelation
### In Caller method of the async method:
- Create a `CancellationTokenSource cts = new CancellationTokenSource()`.
- Send `cts.Token` as input to the async method that can be cancelled.
- Cancel an event by calling `cts.Cancel()`
- `catch (OperationCanceledException)`

### In the Async Method itself:
- Take a `CancellationToken` as input.
- Can check if `IsCancellationRequested()` and perform any necessary cleanup.
- Then can call `ThrowIfCancellationRequested()`, which will throw an `OperationCanceledException`
