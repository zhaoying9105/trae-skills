---
name: transformer-performance-expert
description: Deep analysis of Transformer architectures (Attention, FFN, MoE) with a focus on performance, data flow, communication, and visual diagramming.
metadata:
  openclaw:
    requires:
      bins: ["python"]
    permissions:
      filesystem: "read"
      shell: "allow"
    tags: ["transformer", "attention", "moe", "performance", "diagram"]
    author: "openclaw"
    version: "1.0.0"
---

# Transformer Performance Expert

## Description
This skill provides specialized analysis for Transformer-based architectures. It deconstructs models into key components (Attention, FFN, MoE, Norm) to analyze their specific performance characteristics, data dependencies, and communication patterns. It excels at visualizing these complex interactions through structured diagrams (Mermaid).

## Usage Scenario
Trigger this skill when:
- Analyzing specific Transformer blocks (e.g., "How does this MoE layer work?", "Analyze this Attention implementation")
- Visualizing data flow and tensor shapes across a model
- Optimizing MoE routing strategies and expert load balancing
- Debugging performance issues specific to Attention (O(N^2)) or FFN/MoE (Memory bandwidth)
- Need to map logical model structure to physical execution steps

## Instructions

### 1. Structural Analysis Perspectives

#### A. Attention Mechanism (The Compute/Memory Hybrid)
- **Algorithm**: Identify variant (MHA, GQA, MQA, MLA). Check complexity (O(N^2) vs Linear).
- **Optimization**: Check for FlashAttention, PagedAttention, Ring Attention.
- **KV Cache**: Analyze cache layout, memory footprint, and eviction policies.
- **Communication**: In TP scenarios, analyze the `All-Reduce` after the output projection.

#### B. FFN / Dense Blocks (The Compute Heavy)
- **Activation**: Identify activation function (GeLU, SwiGLU) and potential fusion (Bias+Gate+Up).
- **Compute Bound**: These layers are typically MatMul-heavy. Analyze arithmetic intensity.
- **Parallelism**: Check Column/Row Linear splitting strategies and the `All-Reduce` synchronization point.

#### C. Mixture of Experts (MoE) (The Bandwidth/Comm Heavy)
- **Routing**: Analyze the router (Top-K). Check for load balancing loss or auxiliary loss.
- **Expert Capacity**: Check for token dropping logic or capacity factors.
- **Communication (EP)**: In Expert Parallelism, analyze `All-to-All` dispatch and combine phases.
- **Memory**: Analyze parameter loading (offloading vs resident) and activation memory.

#### D. Normalization & Residuals
- **Pre/Post Norm**: Identify placement (Pre-Norm is standard for stability).
- **Fusion**: Check for RMSNorm/LayerNorm fusion.

### 2. Visualization & Diagramming (Mermaid)
Generate diagrams to visualize the flow. Use the following types:
- **Flowchart**: For data flow and shape transformations (B, S, H).
- **Sequence Diagram**: For communication steps (TP/EP) across ranks.
- **Class Diagram**: For module hierarchy.

### 3. Execution Flow
1. **Block Identification**: Locate the repeated Transformer Layer.
2. **Component Isolation**: Analyze Attention and FFN/MoE separately.
3. **Data Flow Tracing**: Track tensor shapes `[Batch, Seq, Hidden]` through the block.
4. **Comm Mapping**: Insert "Comm Ops" (All-Reduce, All-to-All) into the flow.
5. **Report & Diagram**: Generate the report with embedded Mermaid diagrams.

## Output Template

> **Note**: This content MUST be saved to a file.
> **Default Path**: `performance-reports/transformer-report-{model_name}-{timestamp}.md`

```markdown
# Transformer Architecture & Performance Report
**Target Model**: {model_name}
**Block Type**: {Dense / MoE / Hybrid}

## Executive Summary
- **Attention Variant**: {MHA/GQA/MQA}
- **FFN Type**: {Dense/MoE} (Experts: {num_experts}, Active: {top_k})
- **Critical Bottleneck**: {Attention-Compute / MoE-Comm / FFN-Memory}

## 1. Attention Analysis
- **Structure**: Heads={num_heads}, KV-Heads={num_kv_heads}, Head-Dim={head_dim}
- **Optimization**: {flash_attn_status}, {rope_implementation}
- **Data Flow**:
```mermaid
graph TD
    Input[Input (B, S, H)] --> QKV[QKV Proj]
    QKV --> Split[Split Heads]
    Split --> Attn[FlashAttention]
    Attn --> Out[Output Proj]
    Out --> Comm[All-Reduce (TP)]
```

## 2. FFN / MoE Analysis
- **Type**: {SwiGLU / GeLU}
- **MoE Strategy**: 
  - **Router**: {routing_logic}
  - **Load Balance**: {aux_loss_check}
  - **Comm Pattern**: {all_to_all_dispatch_combine}
- **Data Flow**:
```mermaid
graph TD
    Res[Residual] --> Norm[RMSNorm]
    Norm --> Router[MoE Router]
    Router --> Dispatch[All-to-All (Dispatch)]
    Dispatch --> Expert[Expert Compute (Top-K)]
    Expert --> Combine[All-to-All (Combine)]
    Combine --> Add[Add Residual]
```

## 3. Communication & Parallelism
- **TP Degree**: {tp_size} (Affects Attention/Dense Linear)
- **EP Degree**: {ep_size} (Affects MoE Dispatch/Combine)
- **Overlap**: {comm_overlap_analysis}

## Recommendations
1. **Critical**: {structure_optimization}
2. **Major**: {kernel_fusion_suggestion}
3. **Minor**: {config_tuning}
```

## Resources
- **Checklist**:
  - [ ] Is GQA used for inference efficiency?
  - [ ] Are MoE experts load-balanced?
  - [ ] Is the All-to-All communication latency hidden?
  - [ ] Are RMSNorm and Residuals fused?
  - [ ] Is the KV Cache layout optimized (e.g., Paged)?
