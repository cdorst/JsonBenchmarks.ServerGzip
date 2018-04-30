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
