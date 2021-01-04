# AsyncAwait - Tim  Corey
Code and wisdom from : https://www.youtube.com/watch?v=2moh18sh5p4

A simple sample app that downloads data from a list of websites and writes out how long the opperation took. 

In the source code (*MainWindow.xaml.cs*) three ways of downloading data from the list of websites is presented. 

## 1. RunDownloadSync()
*Total execution time ~ 2.2s*

Simply loops throught the list and calls the synchronous download method for each of the websites.

Advantages:
- very simple to implement

Disadvantages:
- Other processes (EG. moving the UI window) is locked while the operation is running.
- Prints result first after every download is finished.


## 2. RunDownloadAsync()
*Total execution time ~ 2.2s*

Uses `await Task.Run(()=> downloadSync())` to make the sync method async and awaitable. It still does the task one by one so the process takes the same amount of time as the sync version BUT with the important difference.

Advantages:

- Other processes are not locked so it is still possible to for instance interact with the UI.
- Results are printed out as they are completed not all at once.

Disadvantages:
- Still slow.

## 3. RunDownloadParallelAsync()
*Total execution time ~ 0.8s*

Instead of looping through list of websites, it instead does the following:
1. Build a list of Task<ReturnType>:
``````C#
var tasks = new List<Task<WebsiteDataModel>>();

foreach (string site in websites)
{
    tasks.Add(DownloadWebsiteAsync(site));
}
```
2. Use `await Task.WhenAll(tasks)` to complete the async tasks in parallel.

Advantages:
- Fast. Does operations in parallel.
- Does not lock UI