# Read File in File System in C#
+ [ReadFile](https://github.com/gtechsltn/ReadFile)
+ [WriteFile](https://docs.google.com/document/d/1ELVEuJ1OdkbzZQtjVBj3o4DeujnREvXKYOV1zpm7LR0)

## StreamReader: ReadToEndAsync
```
public async Task<string> ReadFileAsync(string filePath)
{
    using (var streamReader = new StreamReader("filePath.txt"))
    {
        string content = await streamReader.ReadToEndAsync();
        // streamReader is automatically disposed when exiting the using block
        return content;
    }
}
```

## StreamReader: ReadLineAsync
```
public async Task ProcessFileAsync(string filePath)
{
    using (var fileStream = new FileStream(filePath, FileMode.Open, FileAccess.Read))
    using (var streamReader = new StreamReader(fileStream))
    {
        string line;
        while ((line = await streamReader.ReadLineAsync()) != null)
        {
            // Process line
        }
    } // fileStream and streamReader are automatically disposed here
}
```

## async await foreach
```
public async IAsyncEnumerable<Product> GetProductsStreamAsync()
{
    await foreach (var product in _dbContext.Products.AsAsyncEnumerable())
    {
        // Simulate some async processing
        await Task.Delay(10);
        yield return product;
    }
}
```

## I/O-bound
```
public async Task<IActionResult> GetOrderDetails(int orderId)
{
    // Await a database call (I/O-bound)
    var order = await _dbContext.Orders.FirstOrDefaultAsync(o => o.Id == orderId);
    if (order == null) return NotFound();

    // Await an external API call (I/O-bound)
    var shippingInfo = await _shippingService.GetShippingDetailsAsync(order.TrackingNumber);

    return Ok(new { Order = order, Shipping = shippingInfo });
}
```
