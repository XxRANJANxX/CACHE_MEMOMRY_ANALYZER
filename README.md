# Cache Simulator with Web UI

A two-level (L1 + L2) cache simulator implemented in C++ with a Python-powered web dashboard for visual analysis and policy comparison.

Supports **LRU**, **FIFO**, **OPT (Optimal / BELADY)**, and **LFU** replacement policies — simulated in parallel from a single memory access trace file.

Trace files are generated using the Intel tool **PIN-TOOL** 

#  Features

- Simulates L1 and L2 caches with configurable size, block size, and associativity
- Four replacement policies compared side-by-side: **LRU**, **FIFO**, **OPT**, **LFU**
- Exports per-policy statistics to `results.csv`
- Web dashboard (Flask) for:
  - Uploading a raw trace file
  - Configuring cache geometry via sliders
  - Viewing hit/miss rates, evictions, access breakdowns
  - Time-series trace histogram
  - Policy comparison across all four algorithms

# Prerequisites

- **C++ compiler** with C++11 or later (e.g. `g++`)
- **Python 3.7+**
- **Flask** — install with:

```bash
pip install flask
```
#  How to Run 

### Step 1 —  Download CACHE.cpp , app.py , index.html keep them in one folder 
# Compile the C++ Simulator

```bash
g++ CACHE.cpp -o cache_sim.exe
```
### Step 2 — Start the Web Server
```bash
python app.py
```

This will start a local Flask server. Open the URL shown in your terminal (typically `http://127.0.0.1:8080`)

# Step 3 — Use the Dashboard
1. **Configure** cache geometry using the sliders (block size, L1/L2 size, associativity)

2. **Upload** your memory access trace file (`.txt`) via the drag-and-drop zone.
    You can select any trace given in the traces folder .

3. Click **RUN C++ SIMULATION**

4. Explore the results across three tabs:
   - **Breakdown** — per-operation hit/miss table and stacked bar
   - **Policy Compare** — LRU vs FIFO vs OPT vs LFU side by side
   - **Time Series Trace** —  **[HIT/MISS]** **[L1/L2/RAM]** **[READ/WRITE]**
  e.g. 
  HIT L2 WRITE --> it is hit in cache and data accessed from L2 and it is write intruction 


# Trace File Format

Each line in the trace file should be:

```
<operation> <hex_address>
```

Example:
```
R 0x1a2b3c
W 0x4d5e6f
R 0x1a2b3c
R 0x1a2b3d
W 0x785e6f
R 0x1ab45c
```

 `R` = read access ,
 `W` = write access

You can take traces generated from mutliple cpp code like dijksta , matrix multiplication , random , linked - list etc in the TRACES folder 

You can also generate trace of your code using PIN-TOOL 

# CLI Usage (Optional)

You can also run the simulator directly from the command line with custom parameters:

```bash
./cache_sim.exe <block_size> <l1_size> <l1_assoc> <l2_size> <l2_assoc>
```

Example:
```bash
./cache_sim.exe 64 1024 2 8192 4
```

Default values (if no arguments given):
| Parameter     | Default |
|---------------|---------|
| Block Size    | 64 B    |
| L1 Size       | 1024 B  |
| L1 Associativ | 2-way   |
| L2 Size       | 8192 B  |
| L2 Associativ | 4-way   |


##  Notes

- The OPT (Optimal/Bélády's) policy requires the full trace to be loaded in advance — it is not realizable in hardware but serves as a theoretical upper bound for comparison.
- Evictions are computed as: `max(0, misses − cache_capacity_in_blocks)` to   exclude cold misses.
