---
date: 2025-02-04
tags:
  - csharp
  - study
source: "[[Videos#Advanced Topics in C Sharp]]"
---
[Asynchronous programming](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/)

In C#, **asynchronous programming** lets you perform tasks in the background without blocking the main thread, which is especially useful for long-running operations like file I/O, network calls, or database queries.

### Normal sync method

```csharp
public static List<WebsiteDataModel> RunDownloadSync()
{
    List<string> websites = PrepData();
    List<WebsiteDataModel> output = new List<WebsiteDataModel>();

    foreach (string site in websites)
    {
        WebsiteDataModel results = DownloadWebsite(site);
        output.Add(results);
    }

    return output;
}
```

```csharp
private static WebsiteDataModel DownloadWebsite(string websiteURL)
{
    WebsiteDataModel output = new WebsiteDataModel();
    WebClient client = new WebClient();

    output.WebsiteUrl = websiteURL;
    output.WebsiteData = client.DownloadString(websiteURL);

    return output;
}
```

### Parallel sync

```csharp
public static List<WebsiteDataModel> RunDownloadParallelSync()
{
    List<string> websites = PrepData();
    List<WebsiteDataModel> output = new List<WebsiteDataModel>();

    Parallel.ForEach<string>(websites, (site) =>
    {
        WebsiteDataModel results = DownloadWebsite(site);
        output.Add(results);
    });

    return output;
}
```

### Async method

```csharp
public static async Task<List<WebsiteDataModel>> RunDownloadAsync(IProgress<ProgressReportModel> progress, CancellationToken cancellationToken)
{
    List<string> websites = PrepData();
    List<WebsiteDataModel> output = new List<WebsiteDataModel>();
    ProgressReportModel report = new ProgressReportModel();

    foreach (string site in websites)
    {
        WebsiteDataModel results = await DownloadWebsiteAsync(site);
        output.Add(results);

        cancellationToken.ThrowIfCancellationRequested();

        report.SitesDownloaded = output;
        report.PercentageComplete = (output.Count * 100) / websites.Count;
        progress.Report(report);
    }

    return output;
}
```

```csharp
private static async Task<WebsiteDataModel> DownloadWebsiteAsync(string websiteURL)
{
    WebsiteDataModel output = new WebsiteDataModel();
    WebClient client = new WebClient();

    output.WebsiteUrl = websiteURL;
    output.WebsiteData = await client.DownloadStringTaskAsync(websiteURL);

    return output;
}
```

#### Caller

```csharp
private async void executeAsync_Click(object sender, RoutedEventArgs e)
{
    Progress<ProgressReportModel> progress = new Progress<ProgressReportModel>();
    progress.ProgressChanged += ReportProgress;

    var watch = System.Diagnostics.Stopwatch.StartNew();

    try
    {
        var results = await DemoMethods.RunDownloadAsync(progress, cts.Token);
        PrintResults(results);
    }
    catch (OperationCanceledException)
    {
        resultsWindow.Text += $"The async download was cancelled. { Environment.NewLine }";
    }

    watch.Stop();
    var elapsedMs = watch.ElapsedMilliseconds;

    resultsWindow.Text += $"Total execution time: { elapsedMs }";
}
```

### Async parallel

```csharp
public static async Task<List<WebsiteDataModel>> RunDownloadParallelAsync(IProgress<ProgressReportModel> progress)
{
    List<string> websites = PrepData();
    List<WebsiteDataModel> output = new List<WebsiteDataModel>();
    ProgressReportModel report = new ProgressReportModel();

    await Task.Run(() =>
    {
        Parallel.ForEach<string>(websites, (site) =>
        {
            WebsiteDataModel results = DownloadWebsite(site);
            output.Add(results);

            report.SitesDownloaded = output;
            report.PercentageComplete = (output.Count * 100) / websites.Count;
            progress.Report(report);
        });
    });

    return output;
}
```

#### Caller

```csharp
private async void executeParallelAsync_Click(object sender, RoutedEventArgs e)
{
    Progress<ProgressReportModel> progress = new Progress<ProgressReportModel>();
    progress.ProgressChanged += ReportProgress;

    var watch = System.Diagnostics.Stopwatch.StartNew();

    var results = await DemoMethods.RunDownloadParallelAsync(progress);
    PrintResults(results);

    watch.Stop();
    var elapsedMs = watch.ElapsedMilliseconds;

    resultsWindow.Text += $"Total execution time: { elapsedMs }";
}
```

***Extra:* Cancel async operation**

```csharp
 CancellationTokenSource cts = new CancellationTokenSource();
...
private void cancelOperation_Click(object sender, RoutedEventArgs e)
{
    cts.Cancel();
}
```

***Extra:* Progress bar**

```csharp
private void ReportProgress(object sender, ProgressReportModel e)
{
    dashboardProgress.Value = e.PercentageComplete;
    PrintResults(e.SitesDownloaded);
}
```
