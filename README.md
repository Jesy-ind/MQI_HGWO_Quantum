MQI_HGWO: code, traces and scripts
Reference code for the paper's connectivity, coverage and convergence optimisation of cluster heads in VANETs using a Modified Quantum-Inspired Hybrid Grey Wolf Optimizer (MQI_HGWO), compared with GWO variants.
> **Important:** simplified VANET model 
Layout
```
MQI_HGWO/
├── README.md
├── LICENSE                (MIT)
├── requirements.txt
├── configs/params.json    simulation parameters and random seed
├── src/
│   ├── gwo_variants.py    MQI_HGWO, GWO and six variants
│   └── network.py         fitness: coverage, connectivity, energy
├── traces/mobility.csv    synthetic grid-road mobility (step, vehicle_id, x, y, speed)
├── scripts/
│   ├── generate_traces.py
│   ├── run_experiments.py metrics -> results/*.csv
│   ├── plot_results.py    results/*.csv -> results/figures/*.png
│   └── run_all.sh         runs all three
└── results/               raw CSVs and figures
```
Run
```bash
pip install -r requirements.txt
./scripts/run_all.sh            # full run, about 30 s
./scripts/run_all.sh --quick    # smoke test
```
Python 3.9+ is required. Seeds are set in `configs/params.json`, so reruns give identical output.
Compared algorithms
`MQI_HGWO`, `GWO`, `I-GWO` (non-linear decay), `OBL-GWO` (opposition-based init), `Levy-GWO`, `Chaotic-GWO` (logistic map init), `DE-GWO`, `PSO-GWO`. The six variants are common, simplified GWO modifications that serve as baselines.
MQI_HGWO keeps a qubit angle per wolf, collapses it to a position, steers it toward the alpha with a rotation gate, applies quantum mutation, and refines the alpha with hybrid GWO local search.
Metrics and output files
File	Content
`results/summary.csv`	fitness, coverage, connectivity, end-to-end delay (ms), residual energy (J), alive nodes, per vehicle count and algorithm
`results/convergence.csv`	best fitness per iteration (middle vehicle count)
`results/alive_nodes.csv`	alive nodes per round
`results/energy_efficiency.csv`	residual energy and residual fraction
`results/figures/`	residual energy, delay, coverage, connectivity, energy efficiency, convergence, alive nodes
Model summary
A solution is the (x, y) positions of K cluster heads in a 1000 m x 1000 m area.
Fitness (lower is better) = 0.4(1 - coverage) + 0.3(1 - connectivity) + 0.3(normalised energy proxy).
Energy uses the first-order radio model (E_elec, E_amp, d^2).
End-to-end delay is a proxy: 2 ms per hop to the sink head plus a small queueing term.
Mobility traces are synthetic. The optimisation uses a static snapshot of vehicle positions; `traces/mobility.csv` is provided for extension to time-varying scenarios (and can be swapped for SUMO output in the same columns).
