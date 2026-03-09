---
name: ai-system-performance-analyzer
description: Analyze AI/LLM code for performance bottlenecks, memory efficiency, parallelism strategies, and operator optimizations.
metadata:
  openclaw:
    requires:
      bins: ["python", "nvcc"]
    permissions:
      filesystem: "read"
      shell: "allow"
    tags: ["performance", "ai", "hpc", "llm"]
    author: "openclaw"
    version: "1.0.0"
---

# AI System Performance Analyzer

## Description
This skill provides deep performance analysis for AI systems, particularly LLM training and inference frameworks. It focuses on identifying inefficiencies in memory usage, compute utilization, parallelism strategies, and communication overhead. It helps optimize model execution by analyzing operator implementations, distributed setups, and resource management.

## Usage Scenario
Trigger this skill when:
- The user asks to "optimize memory usage" or "speed up inference/training"
- Analyzing LLM frameworks (vLLM, TGI, Megatron-LM) for performance bottlenecks
- Debugging slow training steps or high latency inference requests
- Evaluating custom CUDA kernels or Triton implementations
- Need to understand distributed parallelism strategies (TP, PP, DP)

## Instructions

### 1. Analysis Perspectives
Analyze the codebase from the following critical angles:

#### A. Memory Hierarchy & Data Movement (NPU/GPU)
- **SRAM/L1/L2 Utilization**: Check if kernels optimize for on-chip memory (SRAM) reuse to minimize HBM access. Look for tiling/blocking strategies.
- **Data Layout & Access Patterns**: Analyze tensor layout (NHWC vs NCHW) for hardware affinity. Check for coalesced access and bank conflicts.
- **DMA & Async Copy**: Verify usage of asynchronous DMA (Direct Memory Access) for HBM<->SRAM transfers to hide latency.
- **Memory Alignment**: Ensure tensor strides and start addresses meet hardware alignment requirements (e.g., 128-byte alignment) to avoid padding overhead.
- **KV Cache Optimization**: Analyze Block Tables, PagedAttention memory layout, and fragmentation handling.

#### B. Compute & Kernel Micro-Architecture
- **Matrix Unit Saturation**: Check if MatMul/Conv ops are utilizing Tensor Cores / Matrix Units efficiently. Look for tile sizes matching hardware intrinsics (e.g., 16x16x16).
- **Vectorization (SIMD)**: Analyze element-wise kernels for proper vectorization width (e.g., float4 loads/stores).
- **Instruction Level Parallelism (ILP)**: Check for dependency chains stalling the pipeline. Look for loop unrolling and software pipelining.
- **Roofline Analysis**: Determine if ops are Compute Bound or Memory Bandwidth Bound. Suggest fusion for Bandwidth Bound ops.
- **Precision & casting**: Minimize casting overhead. Enforce storage in low precision (FP8/INT8) and compute in high precision (FP32/BF16) only when necessary.

#### C. Operator & Module Macro-Architecture
- **Scaling Bottlenecks**: Analyze how operator performance scales with input size (e.g., Softmax is bandwidth bound at large V, MatMul is compute bound at large batch).
- **Advanced Fusion**: Look beyond element-wise fusion. Check for FlashAttention (Attention+Softmax+Dropout) or Layernorm+Gemm fusion.
- **Module Partitioning (TP/SP)**: Analyze how large modules (MLP, Attention) are split across devices (Column/Row Parallel). Check for "All-Gather" vs "Reduce-Scatter" overhead introduced by splitting.
- **Communication-Computation Trade-off**: Evaluate if splitting a module introduces more communication cost than the compute time saved.
- **Data Flow & Re-materialization**: Check if intermediate activations are re-computed (Gradient Checkpointing) vs stored. Analyze the cost/benefit for specific module sizes.

#### D. Parallelism & Communication Topology
- **Topology Awareness**: Check if communication primitives (`all_reduce`) respect physical topology (NVLink/PCIe switch).
- **Collective Optimization**: Analyze if hierarchical collectives (local-reduce then global-reduce) are used.
- **Overlap & Pipelining**: Verify computation-communication overlap (e.g., backward computation overlapping with gradient sync).
- **Scheduling**: Check for static vs dynamic scheduling in Pipeline Parallelism (1F1B vs Gpipe).

#### E. Inference & Runtime Optimization
- **Kernel Fusion**: Identify opportunities for vertical (Conv+Bias+Relu) and horizontal (Multi-Head Attention) fusion.
- **Graph Execution**: Verify usage of CUDA Graphs / NPU Graph Runtime to eliminate CPU launch jitter.
- **Continuous Batching**: Analyze request scheduling, preemption logic, and memory manager efficiency.
- **Sampler Optimization**: Check if sampling (top-k/top-p) is fused or efficiently implemented to avoid host-device synchronization.

### 2. Execution Flow & Profiling
1. **Critical Path Tracing**: Identify the "Hot Path" (e.g., Attention mechanism, MLP block).
2. **Stall Analysis**: Look for "bubbles" in the pipeline or gaps in device utilization trace.
3. **Hardware Counter Check**: If profiling data exists, check metrics like `sm_efficiency`, `dram_throughput`, `pcie_throughput`.
4. **Report Generation**: 
   - Compile all findings into a Markdown file.
   - **Default Path**: `performance-reports/report-{module_name}-{timestamp}.md`.
   - Use the template below for the file content.

## Output Template

> **Note**: This content MUST be saved to a file, not just printed in the chat.

```markdown
# NPU/AI System Expert Performance Report
**Generated On**: {date}
**Target Module**: {module_or_model_name}

## Executive Summary
- **Bottleneck Classification**: {Memory Bound / Compute Bound / Latency Bound / Comm Bound}
- **Hardware Utilization**: {Low/Medium/High} (Estimated)

## 1. Macro-Architecture & Module Analysis
- **Operator Scaling**: {analysis_of_op_bottlenecks_at_scale}
- **Partitioning Strategy**: 
  - {module_split_analysis_TP_SP}
  - {communication_cost_vs_compute_gain}
- **Advanced Fusion**: {flash_attn_layernorm_fusion_status}

## 2. Micro-Architecture Analysis
- **Memory Hierarchy**:
  - SRAM Reuse: {analysis_of_tiling_caching}
  - Data Layout: {layout_alignment_check}
- **Compute Efficiency**:
  - Vectorization: {simd_width_check}
  - Matrix Utilization: {tensor_core_usage}

## 3. Kernel & Operator Optimization
- **Fusion Status**:
  - {identified_fusion_opportunities}
- **Launch Overhead**: {graph_capture_status}
- **Precision**: {mixed_precision_casting_check}

## 4. Inference Specifics
- **Attention**: {flash_attn_paged_attn_details}
- **Batching**: {continuous_batching_efficiency}
- **Decoding**: {speculative_decoding_status}

## 5. Distributed & System Level
- **Topology**: {rank_mapping_topology_check}
- **Communication**: {overlap_efficiency}

## Expert Recommendations
1. **Critical (Architecture)**: {structural_change_suggestion}
2. **Major (Kernel)**: {kernel_optimization_suggestion}
3. **Minor (Code)**: {implementation_fix}
```

## Resources
- **Checklist**:
  - [ ] Are kernels tiling data for SRAM reuse?
  - [ ] Is data layout aligned (128-byte/padding)?
  - [ ] Are Tensor Cores/Matrix Units saturated?
  - [ ] Is Graph Capture used to hide launch latency?
  - [ ] Are communication ops overlapped with compute?
  - [ ] Is FlashAttention used with correct block sizes?
