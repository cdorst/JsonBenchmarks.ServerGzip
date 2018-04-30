# ASP.NET Core 2.1 API entity data-serialization benchmarks with GZIP response compression
Run `./run.ps1` or `./run.sh` at the repository root to repeat the experiment

## Question

What is the most performant method of data serialization for resources served by ASP.NET Core 2.1 APIs while using GZIP response compression?

## Variables

Three categories of serialization are tested:

- JSON (using GZIP response compression)
- CSV (using GZIP response compression)
- byte[] (GZIP response compression not applicable)

Jil IActionResult, Jil Formatter, and Newtonsoft (default) JSON performance is compared. byte[] object-result vs. IActionResult performance is also compared.

## Hypothesis

Based on the [github.com/cdorst/JsonBenchmarks.Server](https://github.com/cdorst/JsonBenchmarks.Server) work, performance is expected to rank in following order: byte[], CSV, JSON; IActionResult, object-result

## Results

``` ini

BenchmarkDotNet=v0.10.14, OS=Windows 10.0.16299.371 (1709/FallCreatorsUpdate/Redstone3)
Intel Core i7-7700HQ CPU 2.80GHz (Kaby Lake), 1 CPU, 8 logical and 4 physical cores
Frequency=2742186 Hz, Resolution=364.6726 ns, Timer=TSC
.NET Core SDK=2.1.300-preview2-008533
  [Host]     : .NET Core 2.1.0-preview2-26406-04 (CoreCLR 4.6.26406.07, CoreFX 4.6.26406.04), 64bit RyuJIT
  Job-WJXHCJ : .NET Core 2.1.0-preview2-26406-04 (CoreCLR 4.6.26406.07, CoreFX 4.6.26406.04), 64bit RyuJIT

LaunchCount=10  

```
|                       Method |     Mean |    Error |   StdDev |   Median | Rank |
|----------------------------- |---------:|---------:|---------:|---------:|-----:|
|                    ByteArray | 849.9 us | 5.799 us | 31.48 us | 847.1 us |    5 |
|        ByteArrayActionResult | 760.8 us | 5.242 us | 32.26 us | 756.4 us |    2 |
|                          Csv | 756.3 us | 5.002 us | 27.53 us | 754.7 us |    1 |
|          JilJsonActionResult | 769.5 us | 5.166 us | 28.74 us | 767.9 us |    3 |
|   JilJsonActionResultNoNulls | 764.1 us | 5.221 us | 29.00 us | 763.8 us |    2 |
|             JilJsonFormatter | 840.3 us | 5.486 us | 27.95 us | 836.8 us |    4 |
| JilJsonFormatterActionResult | 844.5 us | 5.861 us | 30.02 us | 845.0 us |    4 |
|                  JsonDefault | 853.9 us | 5.947 us | 32.29 us | 850.0 us |    5 |
|      JsonDefaultActionResult | 855.8 us | 5.260 us | 28.20 us | 854.3 us |    5 |

### API Response Time

Jil JsonActionResult type endpoint responds 10.97% faster than default JsonFormatter endpoint

Jil JsonActionResult type without null values endpoint responds 11.75% faster than default JsonFormatter endpoint

CSV endpoint responds 12.90% faster than default JsonFormatter endpoint

byte[] endpoint responds 12.24% faster than default JsonFormatter endpoint

### API Response Content Length

Jil JSON without null values content length is 1.6x smaller (contains 63.16% as many bytes; 36.84% fewer bytes) than default JSON response

CSV content length is 7.6x smaller (contains 13.16% as many bytes; 86.84% fewer bytes) than default JSON response

byte[] content length is 14.3x smaller (contains 7.02% as many bytes; 92.98% fewer bytes) than default JSON response


## Conclusion

Except for CsvActionResult's slightly-faster request-response runtime, byte[]-serialized IActionResult outperformed other methods in terms of data-size, serialization runtime, and API server request-response runtime.

The resultant Data Table indicates that the ASP.NET Core server is less performant in handling object results (with or without a Formatter attribute) than when handling IActionResults

## Future Research

Benchmark client-server scenario (w/ result de-serialization) using strongly-typed C# and TypeScript client SDKs

