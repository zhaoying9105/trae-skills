---
name: ai-system-architecture-expert
description: System-level AI analysis: Operator scaling, Module partitioning, Distributed communication, and Macro-Architecture optimization.
metadata:
  openclaw:
    requires:
      bins: ["python"]
    permissions:
      filesystem: "read"
      shell: "allow"
    tags: ["system", "architecture", "distributed", "tp", "pp", "llm"]
    author: "openclaw"
    version: "1.0.0"
---

# AI System Architecture Expert

## Description
This skill provides macro-level analysis for large-scale AI systems. It focuses on "Operator & Above" optimizations, targeting operator scaling, module partitioning (TP/SP/PP), distributed communication topology, and data flow architecture. It helps optimize how models are split, scheduled, and executed across multiple devices.

## Usage Scenario
Trigger this skill when:
- Analyzing distributed training/inference strategies (Megatron, DeepSpeed)
- Evaluating model partitioning overhead (All-Gather vs Reduce-Scatter)
- Optimizing communication-computation overlap
- Designing large-scale module hierarchies (e.g., "How to split a 70B model?")
- Debugging system-wide stalls or load imbalance

## Instructions

### 1. Analysis Perspectives (Macro-Architecture)

#### A. Operator & Module Scaling
- **Scaling Analysis**: How does operator performance change with `Batch Size` or `Sequence Length`? (e.g., Attention is O(N^2), MLP is O(N)).
- **Bandwidth vs Compute**: Identify which modules become memory-bound vs compute-bound as model size increases.
- **Advanced Fusion**: Look for module-level fusion (e.g., FlashAttention fusing Drop+Softmax+Attn, or Layernorm+Gemm).

#### B. Partitioning & Parallelism
- **Tensor Parallelism (TP)**: Analyze splitting of Linear layers (Column/Row). Check communication cost (All-Reduce) vs compute gain.
- **Sequence Parallelism (SP)**: Check if LayerNorm/Dropout are split along the sequence dimension to save memory.
- **Pipeline Parallelism (PP)**: Evaluate stage balance. Check for "pipeline bubbles" (idle time) in 1F1B or Gpipe schedules.
- **Data Parallelism (DP/FSDP)**: Analyze gradient synchronization overhead and bucket sizes.

#### C. Communication Topology
- **Topology Awareness**: Does the communication pattern match physical links (NVLink vs PCIe)?
- **Collective Efficiency**: Are hierarchical collectives used? Is `all_gather` preferred over `all_reduce` where appropriate?
- **Overlap**: Check if communication is hidden behind backward/forward computation.

#### D. Inference System Flow
- **Continuous Batching**: Analyze request scheduling and memory management (Block Tables).
- **Speculative Decoding**: Check for draft model integration.
- **KV Cache Architecture**: Analyze PagedAttention usage and cache eviction policies.

### 2. Execution Flow
1. **System Map**: Identify the distributed strategy (TP size, PP size, DP size).
2. **Critical Path**: Trace the end-to-end flow of a single training step or inference request.
3. **Bottleneck ID**: Classify as **Communication Bound**, **Memory Capacity Bound**, or **Scheduling Bound**.
4. **Report**: Generate system architecture report.

## Output Template

> **Note**: This content MUST be saved to a file.
> **Default Path**: `performance-reports/system-report-{module_name}-{timestamp}.md`

```markdown
# AI System Architecture Report
**Target System**: {module_name}
**Strategy**: TP={tp_size}, PP={pp_size}, DP={dp_size}

## Executive Summary
- **Primary Bottleneck**: {Comm / Memory Capacity / Scheduling}
- **System Efficiency**: {model_flops_utilization_estimate}

## 1. Module Partitioning Analysis
- **Operator Scaling**: {analysis_of_bottlenecks_at_scale}
- **Splitting Strategy**: 
  - {linear_layer_split_analysis}
  - {norm_dropout_split_analysis}
- **Comm Overhead**: {communication_cost_vs_compute_gain}

## 2. Distributed Communication
- **Topology Fit**: {physical_link_utilization}
- **Overlap Status**: {comm_compute_overlap_check}
- **Collective Ops**: {all_reduce_efficiency}

## 3. Inference/Runtime (If Applicable)
- **Batching**: {continuous_batching_efficiency}
- **KV Cache**: {paged_attention_utilization}

## 4. System Recommendations
1. **Critical (Topology)**: {rank_mapping_fix}
2. **Major (Partitioning)**: {tp_sp_strategy_adjustment}
3. **Minor (Config)**: {bucket_size_tuning}
```

## Resources
- **Checklist**:
  - [ ] Is communication overlapped with computation?
  - [ ] Are linear layers correctly split (Col/Row) to minimize All-Reduce?
  - [ ] Is Sequence Parallelism used for LayerNorm?
  - [ ] Are pipeline bubbles minimized?
  - [ ] Is FlashAttention enabled?
