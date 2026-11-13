# FIO-Based Database Storage Benchmark

## 1. Project Overview

This project evaluates storage performance using Flexible I/O Tester (FIO).
PostgreSQL is selected as the target database. FIO generates synthetic storage I/O; it does not execute SQL queries or reproduce PostgreSQL internals.

## 2. Environment

- Operating System: Windows
- FIO Version: 3.43
- I/O Engine: windowsaio
- Test File: 1 GiB
- I/O Mode: Direct I/O
- Number of Jobs: 1
- Runtime: 45 seconds
- Ramp-up Time: 5 seconds
- Queue Depths: 1, 8, 32
- Repetitions: 3 per workload and queue depth

The experiment was performed natively on Windows using windowsaio instead of the Linux libaio engine.

## 3. Workloads

| ID | Workload | Block Size | Read Percentage |
|---|---|---|---|
| A | Random Read | 4 KiB | 100% |
| B | Random Read/Write | 4 KiB | 70% |
| C | Random Read/Write | 8 KiB | 30% |
| D | Sequential Read | 256 KiB | 100% |

Total benchmark runs: 36.

## 4. Project Structure

- data: 1 GiB test file
- workloads: Four FIO configuration files
- results: 36 JSON files and two CSV files
- graphs: Four comparison graphs
- report: Final PDF report

## 5. Performance Metrics

- IOPS: Input/output operations per second.
- Bandwidth: Data transferred per second.
- Average Latency: Mean completion latency.
- P50: Median completion latency.
- P95: 95th percentile completion latency.
- P99: 99th percentile completion latency.

Bandwidth is expressed in MiB/s and latency in microseconds.

The percentile values in the summary are arithmetic averages of the per-run percentiles, not pooled percentiles.

## 6. Running Workloads

Open PowerShell:

```powershell
cd D:\Lab10_FIO
& "C:\Program Files\fio\fio.exe" workloads\A_random_read.fio
& "C:\Program Files\fio\fio.exe" workloads\B_mixed_70read.fio
& "C:\Program Files\fio\fio.exe" workloads\C_mixed_30read.fio
& "C:\Program Files\fio\fio.exe" workloads\D_sequential_read.fio

```
## 7. Results

The results folder contains 36 individual FIO JSON output files.

- raw_metrics.csv: Individual-run measurements.
- summary_metrics.csv: Average measurements grouped by workload, queue depth and I/O direction.

The graphs folder contains four comparison graphs: IOPS, bandwidth, average latency and P99 latency.

## 8. PostgreSQL Context and Limitations

PostgreSQL is the selected database. The workloads approximate storage access patterns relevant to database systems, including small random reads, mixed reads and writes, and large sequential reads.

FIO does not execute SQL statements or model PostgreSQL buffer management, transactions, indexes, WAL, query planning, locking or database caching.

Therefore, these results represent synthetic storage performance under the tested Windows environment, not actual PostgreSQL benchmark performance.

## 9. Reproducibility

Test file:
D:\Lab10_FIO\data\db-test.bin

FIO executable:
C:\Program Files\fio\fio.exe

The workload configurations use the same test file. The original experiment results are retained in the results folder.
