# Sorting Algorithms: Performance, Complexity & Stability

**ST1507 Data Structures and Algorithms (CA2) | Singapore Polytechnic**  
**Python · Jupyter Notebook · pandas · NumPy · Matplotlib**

## Project Overview

This academic project investigates how sorting algorithms behave in practice and how algorithm choice depends on **input size, data arrangement, computational complexity, memory usage, and sorting stability**.

In a two-person collaboration, we implemented and evaluated four comparison-based sorting algorithms—**Bubble Sort, Merge Sort, Quick Sort, and Heap Sort**—using controlled benchmarks and data visualisations. The notebook also contains two separately attributed individual investigations: **Sorting Stability** (my investigation) and **In-Place Merge Sort** (my teammate's investigation).

The aim was not simply to identify the "fastest" algorithm, but to explain the trade-offs behind each choice using measurable evidence.

## Tools & Technologies

| Tool / Concept | How it was used |
|---|---|
| **Python** | Implemented sorting algorithms, assertions, and experiments |
| **Jupyter Notebook** | Documented implementations, outputs, findings, and explanations |
| **pandas & NumPy** | Organised benchmark results and supported numerical analysis |
| **Matplotlib** | Plotted comparison growth and input-dependent performance |
| **`time.perf_counter()`** | Measured supplementary execution-time results |
| **`tracemalloc`** | Measured peak traced memory in the teammate's in-place investigation |
| **Big-O analysis** | Related observed behaviour to time and auxiliary-space complexity |

## Project Structure

```text
sorting-algorithms-performance-analysis/
├── README.md
├── sorting_algorithms_analysis.ipynb
├── requirements.txt
└── presentation.pdf
```

The notebook contains the following investigations:

1. **Part A — Bubble Sort vs Merge Sort** (collaborative)
2. **Part B — Quick Sort vs Heap Sort** (collaborative)
3. **Individual — In-Place Merge Sort** (teammate's investigation)
4. **Individual — Sorting Stability / Selection Sort** (my investigation)

## Methodology

- Implemented comparison-counting versions of the sorting algorithms.
- Benchmarked datasets ranging from **5 to 100,000 elements** for the core algorithm comparisons.
- Compared multiple input arrangements: **random, already sorted, reverse sorted, all identical, few unique values, and almost sorted**.
- Gave competing algorithms separate copies of the **same input** and checked sorted outputs against Python's `sorted()`.
- Used fixed random seeds for reproducible datasets.
- Visualised raw and normalised comparison counts, including log-scale plots, to assess growth relative to **O(n²)** and **O(n log n)**.
- Examined practical trade-offs rather than relying on asymptotic complexity alone.

> **Measurement note:** Comparison counts measure explicitly instrumented *data-value comparisons* in these implementations, not a universal runtime ranking. Runtime results may vary by machine. Some notebooks cells—especially Bubble Sort at 100,000 elements—are computationally expensive to rerun.

## Selected Results

### 1. Bubble Sort vs Merge Sort

At **n = 100,000**, the recorded number of comparisons was approximately:

| Algorithm | Comparisons | Expected growth |
|---|---:|---|
| Bubble Sort | 9.97 billion | O(n²) average/worst case |
| Merge Sort | 1.54 million | O(n log n) |

**Finding:** Bubble Sort performed approximately **6,490× more comparisons**, demonstrating how quickly quadratic growth becomes costly at large input sizes. Optimised Bubble Sort can still terminate early for already-sorted data.

### 2. Quick Sort vs Heap Sort

The implementations compared were **median-of-three, three-way Quick Sort** and **binary max-heap Heap Sort**.

| Random dataset (n = 100,000) | Comparisons |
|---|---:|
| Quick Sort | 2,813,769 |
| Heap Sort | 3,019,828 |

**Finding:** Quick Sort required approximately **6.8% fewer comparisons** on this random input, whereas Heap Sort offers a guaranteed **O(n log n)** worst-case time complexity with **O(1)** auxiliary space in the iterative implementation. Three-way partitioning was particularly effective on inputs with many duplicate values.

### 3. My Individual Investigation — Sorting Stability

**Research question:** What does it cost to preserve the original relative order of records with equal sorting keys?

I compared:

- **Standard Selection Sort:** moves the minimum record using a direct swap, which can change the order of equal-key records.
- **Stable Selection Sort:** moves the minimum through **shifting and insertion**, preserving equal-key order.

I first checked stability using labelled records with equal keys, then benchmarked *record writes* and comparisons under different input arrangements.

At **n = 500**, both versions used **124,750 key comparisons**, but record movement differed:

| Input arrangement | Standard record writes | Stable record writes |
|---|---:|---:|
| Already sorted | 0 | 0 |
| Almost sorted | 2 | 2 |
| Random | 988 | 63,244 |
| Reverse sorted | 500 | 125,249 |

**Key finding:** The stable version required roughly **64× more record writes** on the tested random input and **250.5× more** on the reverse-sorted input. Both versions retain **Θ(n²)** comparison complexity and **O(1)** auxiliary space, but their data-movement costs are very different.

**Conclusion:** Stability has practical value for workflows involving equal-key records, but its cost depends on the modification used and the input arrangement. The 250.5× figure is a **specific high-movement case**, not a universal overhead.

### 4. Additional Notebook Content — Teammate's Individual Investigation

The notebook also includes a separate study of **standard vs shift-based in-place Merge Sort**, exploring the trade-off between lower auxiliary memory and increased element movement/runtime. **This was my teammate's individual investigation**, included here for completeness and attribution.

## Key Takeaways

- **Algorithm scalability matters:** a solution that works for small datasets may become impractical as input size grows.
- **Input arrangement matters:** sorted, reverse-sorted, and duplicate-heavy inputs can change operation counts significantly.
- **Worst-case guarantees matter:** Quick Sort and Heap Sort present different trade-offs even when their average-case complexity appears similar.
- **Stability matters:** preserving equal-key order can require substantially more record movement, even when the Big-O comparison complexity is unchanged.
- **Benchmarks need careful interpretation:** comparison counts, record writes, memory and runtime capture different aspects of performance.

## How to Run

1. Clone or download this repository.
2. Install **Python 3** and Jupyter Notebook or JupyterLab.
3. Install the project packages:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Open `sorting_algorithms_analysis.ipynb` in Jupyter Notebook, JupyterLab, or VS Code.
5. Run selected sections in order. The initial setup cells define shared test data and functions required by later experiments.

> **Important:** A full run includes quadratic-time benchmarks with large datasets, so it can take considerable time. Review the notebook's saved results first and consider reducing the largest benchmark sizes for a quick local demonstration. Reducing sizes will produce different numerical results from those reported above.

## Limitations

- Benchmark measurements reflect the specific Python implementations and tested datasets.
- One fixed seed per selected experiment supports reproducibility, but does not establish an average over many random seeds.
- The stability investigation reports *logical record-write counts*, not bytes moved or production-system runtime.
- The stable Selection Sort experiment with unique keys isolates movement cost; equal-key stability is verified separately.
- Different in-place merging techniques may yield different memory/runtime trade-offs.

## Presentation

See [`presentation.pdf`](presentation.pdf) for the project's key figures and findings. The presentation should be **redacted of student identification numbers before publishing**, and team members should agree to public sharing.

---

*Academic coursework project. Collaborative analyses and individual investigations are attributed separately above.*
