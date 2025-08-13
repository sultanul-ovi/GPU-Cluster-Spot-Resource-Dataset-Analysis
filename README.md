# GPU Cluster Spot Resource Dataset Analysis

## Detailed Analysis Traces for AI jobs leveraging spot GPU resources

This dataset provides a comprehensive trace of AI workloads running on a large-scale GPU cluster with spot resource provisioning capabilities. It captures real-world operational characteristics from a production environment managing both high-priority workloads with strict Service Level Objectives (SLOs) and opportunistic spot workloads.

### Key Characteristics

- **Infrastructure Scale**: 4,278 GPU nodes with 6 different GPU card types
- **Workload Volume**: 466,867 job submissions tracked
- **Organization Diversity**: 119 unique organizations/departments
- **Workload Types**: Mixed high-priority (HP) and spot instance workloads

## 🗄️ Dataset Details

### Field Descriptions of the file `node_info_df.csv`

- `node_name`: Unique identifier for the node.
- `gpu_model`: Type of GPU cards. For example, A10, A100-SXM4-80GB, etc.
- `gpu_capacity_num`: GPU capacity of the node. A node may exhibit heterogeneous GPU configurations. For example, `node_0` have `4` GPUs and `node_3` have `8` GPUs, etc.
- `cpu_num`: Number of CPU cores in a node (in vCPUs).

### Field Descriptions of the file `job_info_df.csv`

- `job_name`: Unique identifier for the job.
- `organization`: Cost organizations, which encompass various administrative units, like departments, agencies, etc.
- `gpu_model`: Type of GPU cards requested by the job.
- `cpu_request`: Number of CPU cores requested by the job (in vCPUs).
- `gpu_request`: Number of GPU requested by the job.
- `worker_num`: Number of instances requested by the job.
- `submit_time`: Timestamp indicating when the job requests, expressed as the difference in seconds from the first submitted job.
- `duration`: Duration of job (in seconds)
- `job_type`: Type of job:
  - High-priority job
  - Spot job

## 🏗️ Infrastructure Profile

### GPU Resources

The cluster demonstrates significant heterogeneity in GPU provisioning:

| GPU Model      | Node Count | Total GPUs | Avg GPUs/Node |
| -------------- | ---------- | ---------- | ------------- |
| A100-SXM4-80GB | 1,424      | 8,544      | 6.0           |
| A10            | 1,427      | 1,427      | 1.0           |
| GPU-series-1   | 1,423      | 1,706      | 1.2           |
| A100-PCIE-40GB | 2          | 16         | 8.0           |
| GPU-series-2   | 1          | 8          | 8.0           |
| A30            | 1          | 1          | 1.0           |

**Total GPU Cards**: 11,702 GPUs across the entire cluster

### Node Configurations

The cluster exhibits 13 unique node configurations, with the most common being:

- Single GPU nodes with 128 or 192 vCPUs (dominant configuration)
- Multi-GPU nodes (4-8 GPUs) primarily for A100 series cards
- Maximum node capacity: 8 GPUs per node

### CPU Resources

- **Total vCPU Capacity**: 673,088 cores
- **Average vCPUs per Node**: 157 cores
- **Configuration Range**: 64-192 vCPUs per node

## 📈 Workload Characteristics

### Job Distribution

- **High-Priority (HP) Jobs**: 345,515 (74.01%)
- **Spot Jobs**: 121,352 (25.99%)

This 3:1 ratio indicates a production environment where guaranteed service dominates but significant opportunistic capacity exists.

### Job Duration Patterns

| Job Type  | Mean Duration | Median Duration | Max Duration |
| --------- | ------------- | --------------- | ------------ |
| HP Jobs   | 8.7 hours     | 0.77 hours      | 244 days     |
| Spot Jobs | 1.8 hours     | 0.28 hours      | 104 days     |

**Key Insights**:

- HP jobs run 4.8× longer on average than spot jobs
- Both job types show high variance (median << mean), indicating diverse workload patterns
- Long-tail distribution suggests mix of interactive and batch processing workloads

### Resource Requirements

#### GPU Demands

- **Single GPU jobs dominate**: 97.8% of all jobs request 1 GPU per worker
- **Multi-GPU jobs**: Primarily A100-based, up to 8 GPUs per worker
- **GPU Model Preferences**:
  - HP jobs: 45% A10, 31% GPU-series-1, 24% A100-SXM4-80GB
  - Spot jobs: 37% A10, 27% GPU-series-1, 36% A100-SXM4-80GB

#### Parallelism Patterns

- **Single-worker jobs**: 88.7% of all submissions
- **Multi-worker jobs**: 11.3%, with spot jobs showing higher parallelism
  - Maximum workers observed: 256 (spot job)
  - Average workers for multi-worker jobs: HP=8.7, Spot=24.3

### Organizational Usage

The top 10 organizations account for 73.5% of all job submissions, indicating concentrated usage patterns typical of shared research/production clusters.

**Top 5 Organizations by Job Volume**:

1. Organization 43: 103,459 jobs (91% HP, 9% Spot)
2. Organization 13: 101,635 jobs (99.9% HP)
3. Organization 15: 74,253 jobs (100% HP)
4. Organization 77: 31,234 jobs (24% HP, 76% Spot)
5. Organization 10: 25,641 jobs (75% HP, 25% Spot)

## 💡 Key Findings and Insights

### 1. Resource Utilization Patterns

- **Total GPU Hours Consumed**: 31.2 million hours
  - HP jobs: 29.8M hours (95.5%)
  - Spot jobs: 1.4M hours (4.5%)
- **GPU Hour Efficiency**: Despite being 26% of jobs, spot workloads consume only 4.5% of GPU hours, demonstrating their opportunistic nature

### 2. Workload Heterogeneity

The dataset reveals extreme diversity in job characteristics:

- Duration range: seconds to months
- Resource scale: single GPU to 256-worker distributed jobs
- Mixed AI/ML workloads: training (long duration) vs inference (short duration)

### 3. Spot Resource Opportunity

- Average spot job uses 85% less GPU time than HP jobs
- High job turnover rate (median < 20 minutes) suggests good spot availability windows
- 36% of spot jobs target premium A100 GPUs, indicating cost-optimization strategies

### 4. Infrastructure Utilization

- Heterogeneous GPU provisioning supports diverse workload requirements
- Node configurations optimized for both single-GPU (inference/development) and multi-GPU (training) workloads
- CPU:GPU ratio (57:1 vCPUs per GPU) indicates CPU-intensive preprocessing capabilities

### 5. Scheduling Complexity Indicators

- 119 organizations competing for resources
- Mixed priority levels with strict SLOs for 74% of workloads
- High variance in job characteristics requires sophisticated scheduling
- Resource fragmentation potential with varying GPU requirements

## 🔬 Research Applications

This dataset is valuable for:

1. **Scheduling Algorithm Development**

   - Spot instance prediction models
   - Multi-resource scheduling optimization
   - SLO-aware preemption strategies

2. **Cluster Design Studies**

   - GPU provisioning optimization
   - Heterogeneous resource planning
   - Cost-performance trade-off analysis

3. **Workload Characterization**

   - AI/ML job pattern analysis
   - Organization behavior modeling
   - Resource demand forecasting

4. **Economic Analysis**
   - Spot pricing strategies
   - Resource allocation fairness
   - Cost optimization for mixed workloads

## 📝 Dataset Limitations and Considerations

1. **Temporal Coverage**: Observation period spans approximately 113 days
2. **Anonymization**: Organization and GPU model names are partially anonymized
3. **Missing Metrics**: No information on job success/failure rates, actual vs requested resources, or pricing
4. **Static Infrastructure**: Node configuration assumed constant throughout observation period

## 🎯 Recommended Analysis Extensions

1. **Temporal Analysis**: Job arrival patterns, peak usage periods, seasonal trends
2. **Failure Analysis**: Spot preemption impact on job completion
3. **Efficiency Metrics**: Resource waste, fragmentation, and utilization rates
4. **Predictive Modeling**: Spot availability forecasting, job duration prediction
5. **Fair Sharing**: Organization-level resource allocation and priority analysis

---

_This dataset represents a significant contribution to the understanding of large-scale GPU cluster operations and spot resource management in production AI/ML environments._
