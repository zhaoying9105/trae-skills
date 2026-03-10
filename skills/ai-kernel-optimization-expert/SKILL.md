---
name: ai-kernel-optimization-expert
description: Deep-dive optimization for AI kernels, operators, and micro-architecture. Focuses on SRAM, vectorization, and NPU intrinsics.
metadata:
  openclaw:
    requires:
      bins: ["nvcc"]
    permissions:
      filesystem: "read"
      shell: "allow"
    tags: ["kernel", "cuda", "triton", "npu", "optimization"]
    author: "openclaw"
    version: "1.0.0"
---

# AI Kernel Optimization Expert

## Description
This skill provides expert-level analysis for low-level AI kernel optimization. It focuses on "Operator & Below" optimizations, targeting micro-architecture efficiency (SRAM, Registers, Tensor Cores), custom kernel implementations (CUDA/Triton), and hardware-specific constraints.

## Usage Scenario
Trigger this skill when:
- Analyzing custom CUDA or Triton kernel implementations
- Profiling individual operator performance (e.g., "Why is this Softmax slow?")
- Optimizing memory access patterns (coalescing, tiling, bank conflicts)
- Evaluating Tensor Core / Matrix Unit saturation
- Debugging numerical precision or casting overhead

## Instructions

### 1. Analysis Perspectives (Micro-Architecture)

#### A. Memory Hierarchy & Data Movement
- **SRAM/L1 Reuse**: Check tiling/blocking strategies. Is data reused enough in registers/shared memory to hide HBM latency?
- **Global Memory Access**: Analyze coalescing (128-byte transactions). Check for unaligned accesses or strided patterns causing bandwidth waste.
- **Bank Conflicts**: Identify shared memory bank conflicts in reduction or transpose kernels.
- **Async Copy**: Verify usage of asynchronous copy instructions (e.g., `cp.async` on NVIDIA, DMA on NPU) to overlap load/compute.

#### B. Compute & Instruction Efficiency
- **Matrix Saturation**: Check tile sizes (e.g., 16x16x16). Are Tensor Cores / Matrix Units being fed efficiently?
- **Vectorization**: Analyze load/store vectorization (e.g., `float4`, `LDS.128`). Check if element-wise ops use SIMD instructions.
- **Pipeline Stalls**: Look for dependency chains. Suggest loop unrolling or software pipelining to hide instruction latency.
- **Reg Spills**: Check for excessive register pressure causing spills to local memory.

#### C. Kernel-Level Fusion & Precision
- **Vertical Fusion**: Identify opportunities to fuse memory-bound ops (Bias, Relu, Residual) into preceding compute-bound ops (MatMul, Conv).
- **Precision Management**: Minimize casting. Ensure accumulation happens in FP32, storage in FP16/BF16/FP8.
- **Launch Overhead**: For small kernels, check if launch latency dominates execution time.

### 2. Execution Flow
1. **Source Analysis**: Read kernel code (`.cu`, `.triton`, or Python JIT).
2. **Intrinsic Check**: Verify usage of hardware intrinsics vs generic math.
3. **Bottleneck ID**: Classify as **Memory Bound** (low arithmetic intensity) or **Compute Bound** (low IPC/Unit utilization).
4. **Report**: Generate detailed optimization report.

## Output Template

> **Note**: This content MUST be saved to a file.
> **Default Path**: `performance-reports/kernel-report-{op_name}-{timestamp}.md`

```markdown
# Kernel Micro-Architecture Report
**Target Operator**: {op_name}
**Hardware Context**: {GPU/NPU_Architecture}

## Executive Summary
- **Bottleneck**: {Memory / Compute / Latency}
- **Theoretical Occupancy**: {analysis_of_occupancy}

## 1. Memory Subsystem
- **Coalescing**: {global_memory_access_analysis}
- **SRAM Efficiency**: 
  - Tiling Strategy: {tile_size_analysis}
  - Bank Conflicts: {conflict_check}

## 2. Compute Pipeline
- **Vectorization**: {simd_width_check}
- **Tensor Core Usage**: {mma_instruction_check}
- **Instruction Stalls**: {dependency_analysis}

## 3. Optimization Recommendations
1. **Critical**: {tiling_or_layout_fix}
2. **Major**: {vectorization_fix}
3. **Minor**: {pragma_unroll_hint}
```

## Resources
- **Checklist**:
  - [ ] Is `float4` / vectorized load used?
  - [ ] Are shared memory bank conflicts minimized?
  - [ ] Are async copy instructions used?
  - [ ] Is the tile size aligned with hardware tensor cores?
  - [ ] Are redundant global memory round-trips eliminated via fusion?
