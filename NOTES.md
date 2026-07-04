## 2. Sales Summary Function for Part 2

The sales summary function in my assignment is the `GenerateSalesReport` function. It creates a report file named `salesReport.txt` that contains the total sales and the sales total for each `sales.json` file.

```csharp
void GenerateSalesReport(IEnumerable<string> salesFiles, double salesTotal, string salesTotalDir)
{
    var report = new StringBuilder();
    report.AppendLine("Sales Summary");
    report.AppendLine("----------------------------");
    report.AppendLine($"Total Sales: {salesTotal:C}");
    report.AppendLine();
    report.AppendLine("Details:");

    foreach (var file in salesFiles)
    {
        try
        {
            string salesJson = File.ReadAllText(file);
            if (string.IsNullOrWhiteSpace(salesJson))
            {
                report.AppendLine($"  {Path.GetFileName(file)}: Empty file");
                continue;
            }

            SalesData? data = JsonConvert.DeserializeObject<SalesData?>(salesJson);
            if (data != null)
            {
                report.AppendLine($"  {Path.GetFileName(file)}: {data.Total:C}");
            }
            else
            {
                report.AppendLine($"  {Path.GetFileName(file)}: Invalid JSON");
            }
        }
        catch (JsonException)
        {
            report.AppendLine($"  {Path.GetFileName(file)}: Error parsing JSON");
        }
        catch (Exception)
        {
            report.AppendLine($"  {Path.GetFileName(file)}: Error reading file");
        }
    }

    File.WriteAllText(Path.Combine(salesTotalDir, "salesReport.txt"), report.ToString());
}