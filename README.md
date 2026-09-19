<p align="center">
  <img src="assets/avatar.jpg" width="140" height="140" alt="AIEN Sovereign Intelligence" style="border-radius: 50%; border: 2px solid #f59e0b;">
</p>

# spark-hive

[![Crates.io](https://img.shields.io/badge/crates.io-v0.1.0-orange.svg)](https://github.com/aien-dev/spark-hive)
[![License: SRCL-1.0](https://img.shields.io/badge/License-SRCL--1.0-blue.svg)](LICENSE)
[![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)](https://github.com/aien-dev/spark-hive)

Native sovereign agent hive engine with 2D axial hexagonal geometry and autonomous consumer model adapter pipeline.

## Overview

`spark-hive` is an autonomous multi-agent coordination engine written in native Rust. It organizes agent thoughts, Socratic inquiries, and subagent task completions onto a spatial 2D axial hexagonal lattice `(q, r)`.

It includes a built-in **Consumer Model Adapter Engine** that benchmarks, optimizes, and generates upstream contribution pull requests for everyday open-weight models running on consumer hardware:
- **Qwen2.5-Coder (1.5B / 7B)**
- **Llama-3.2 (1B / 3B)**
- **Gemma-2 (2B / 9B)**
- **DeepSeek-R1-Distill (1.5B / 7B / 8B)**

## Features

- **Hexagonal Axial Geometry**: Implements 2D axial coordinates `(q, r)` with six deterministic neighbor vectors, Euclidean pixel conversions, and Manhattan hex distance algorithms.
- **Collision-Free Atomic Storage**: SQLite-backed lattice storage with connection-level mutexes for high-concurrency multi-threaded agent emission.
- **Dynamic Adapter Pipeline**: Automatically estimates parameter-scaled inference throughput (tok/s), memory footprints, and time-to-first-token (TTFT) across consumer hardware profiles (8GB laptops to 32GB workstations).
- **Socratic Grounding Reflex**: Evaluates architectural contributions against technical merit and open-weight accessibility.
- **Traceable Honeycomb Chains**: Visualizes 4-stage pipeline execution directly on the hexagonal wall.

## Coordinate System

The engine uses standard axial coordinates `(q, r)` mapped to the six primary neighbor vectors:

| Direction | dq | dr | Label |
| :--- | :--- | :--- | :--- |
| 0 | +1 | 0 | East |
| 1 | +1 | -1 | Northeast |
| 2 | 0 | -1 | Northwest |
| 3 | -1 | 0 | West |
| 4 | -1 | +1 | Southwest |
| 5 | 0 | +1 | Southeast |

Pixel mapping follows the flat-topped hexagonal layout with default radius R = 88px:
- x = R * sqrt(3) * (q + r / 2)
- y = R * 1.5 * r

## Quickstart

Add to your `Cargo.toml`:

```toml
[dependencies]
spark-hive = { git = "https://github.com/aien-dev/spark-hive.git" }
```

### Basic Placement Example

```rust
use spark_hive::store::{CombStore, PlaceCombInput};

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let store = CombStore::open_in_memory()?;
    
    // 1. Place genesis comb at origin (0, 0)
    let genesis = store.place_comb(PlaceCombInput {
        q: Some(0),
        r: Some(0),
        author: "AIEN".into(),
        role: Some("coordinator".into()),
        content: "Sovereign Hive Initialized".into(),
        intent: Some("independent".into()),
        parent_id: None,
    })?;

    // 2. Auto-place adjacent neighbor linked to genesis
    let child = store.place_comb(PlaceCombInput {
        q: None,
        r: None,
        author: "AIEN (Subagent)".into(),
        role: Some("researcher".into()),
        content: "Consumer adapter pipeline validated".into(),
        intent: Some("join".into()),
        parent_id: Some(genesis.id),
    })?;

    println!("Placed child comb at coordinates ({}, {})", child.q, child.r);
    Ok(())
}
```

### Socratic Adapter Evaluation

```rust
use spark_hive::adapter_engine::{find_or_create_adapter, ConsumerModel, UpstreamEngine};

fn main() {
    let spec = find_or_create_adapter(ConsumerModel::Qwen2_5Coder1_5B, UpstreamEngine::Candle);
    println!("Adapter: {}", spec.title);
    println!("Estimated Latency: {}ms TTFT", spec.telemetry.time_to_first_token_ms);
}
```

## Testing

Run the test suite:

```bash
cargo test
```

Verification covers:
- Concurrent placements across 12 simultaneous threads (`test_concurrent_auto_place_comb`)
- Axial coordinate Euclidean round-tripping (`test_axial_to_pixel_and_back_roundtrip`)
- Parameter scaling estimations (`test_telemetry_estimate_for_model_scaling`)
- Socratic evaluation gate checks (`test_socratic_evaluation_approval`)

## Authors

- **AIEN** (`aien.atlas@proton.me`)
- **AIEN Sovereign Contributors**

## License and Governance

Licensed under the **Sovereign Resource Commons License 1.0 (SRCL-1.0)** (Apache-2.0 WITH LLVM-exception).
Architected by AIEN (Autonomous Cognitive Architecture operating on the Atlas Framework) and sovereign ecosystem contributors. See [LICENSE](LICENSE) for full legal terms and copyright notices.

All downstream distributions, derivative works, and commercial deployments are governed exclusively by the terms of [LICENSE](LICENSE). [CONSTITUTION.md](CONSTITUTION.md) defines the internal architectural charter and development doctrine for upstream engineering.
