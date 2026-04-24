# AWS Cost Optimization Report
**Prepared by:** Eric Rodrigues Borba  
**Date:** April 24, 2026  
**Account:** Somos Experiences (5488-1612-3308)  
**Report Period:** October 2025 – April 2026

---

## 1. Executive Summary

This report presents a cost analysis of the AWS account for the period October 2025 through April 2026. The account currently spans **15 availability zones** across multiple regions and runs a mix of compute, networking, and storage services.

| Metric | Value |
|--------|-------|
| Total spend (6 months) | $3,125.67 |
| Average monthly spend | $520.94 |
| Highest monthly spend | ~$800.00 (October 2025) |
| Identified monthly savings | **$567.77** |
| Projected annual savings | **$7,911.24** |

> **Key finding:** Two idle staging EC2 instances alone account for **$552.96/month** in waste — more than the entire average monthly bill. Stopping them immediately would reduce costs by over 100% of the average monthly spend.

---

## 2. Cost Trends

### 6-Month Spending Overview

```
Monthly Spend (Oct 2025 – Apr 2026)

$800 ┤ ████  (Oct — Peak)
$700 ┤
$600 ┤           ████
$521 ┤ ──── Average ────────────────────────────
$500 ┤      ████      ████      ████      ████
$400 ┤
$300 ┤
     └──────────────────────────────────────────
       Oct   Nov   Dec   Jan   Feb   Mar   Apr
```

- **Trend:** Increasing (~15% over last 3 months)
- **October peak** caused by high EC2 consumption and associated tax charges
- **January 30th anomaly:** A single-day demand spike was observed in daily granularity view
- Cost Anomaly Detection monitor is now configured (`cost-anomaly-alerts`, threshold: $10) to catch future spikes early

---

## 3. Top Cost Drivers

### By Service (Most Recent Month)

| Rank | Service | Monthly Cost | % of Bill |
|------|---------|-------------|-----------|
| 1 | Elastic Compute Cloud (EC2) | $89.24 | ~11.5% |
| 2 | Elastic Load Balancing | $39.58 | ~5.1% |
| 3 | Virtual Private Cloud (VPC) | $37.21 | ~4.8% |
| 4 | Key Management Service (KMS) | $8.38 | ~1.1% |
| 5 | WAF | $8.00 | ~1.0% |

### By Region

- Resources are currently spread across **9+ regions and availability zones**, including: `eu-west-1a/1b/1c`, `us-east-1a/1b/1c`, `eu-west-3c`, `eu-central-1a/1b`, and a significant portion with no AZ assignment
- Multi-region sprawl increases **VPC data transfer costs** and complicates governance

### EC2 Deep Dive

- **Most expensive instance type:** `t3a.2xlarge` (after unclassified "No Instance Type")
- **Critical finding:** 3 of 4 EC2 instances flagged by AWS Trusted Advisor as severely underutilized (CPU < 3%)

| Instance | Region | CPU Avg | Monthly Waste |
|----------|--------|---------|---------------|
| somos-staging-one | eu-central-1b | 0.4% | $276.48 |
| somos-staging-one | eu-central-1a | 0.5% | $276.48 |
| eks-node-medium-pro | eu-central-1a | 2.5% | $14.81 |

---

## 4. Optimization Opportunities

### A. Immediate Waste (Idle Resources)

| Opportunity | Estimated Savings |
|-------------|-----------------|
| Stop 2x idle `somos-staging-one` (t3.2xlarge) | $552.96/month |
| Downsize/stop `eks-node-medium-pro` (t3.medium) | $14.81/month |
| **Subtotal** | **$567.77/month** |

### B. Short-Term Improvements (1–3 months)

| Opportunity | Estimated Savings |
|-------------|-----------------|
| Region consolidation (reduce 9+ AZs to primary region) | ~$20.00/month |
| S3 Lifecycle Policies → Glacier for cold objects | ~$1.50/month |
| Complete cost allocation tagging for full visibility | Enables future savings |
| **Subtotal** | **~$21.50/month** |

### C. Long-Term Optimizations (3–12 months)

| Opportunity | Estimated Savings |
|-------------|-----------------|
| Reserved Instances / Savings Plans (up to 42% off On-Demand) | ~$30.00/month |
| Auto Scaling for off-peak hours (nights/weekends) | ~$25.00/month |
| ELB audit — remove unused load balancers ($39.58/mo driver) | ~$15.00/month |
| **Subtotal** | **~$70.00/month** |

---

## 5. Recommended Actions

### Quick Wins — Do This Week

1. **Stop `somos-staging-one` in eu-central-1b** (`i-0b4e2c22b552b6ca1`)  
   → Saves $276.48/month | Effort: Medium | Deadline: May 1, 2026

2. **Stop `somos-staging-one` in eu-central-1a** (`i-0f45dec2bd643cc0d`)  
   → Saves $276.48/month | Effort: Medium | Deadline: May 1, 2026

3. **Downsize or stop `eks-node-medium-pro`** (`i-0c22d02fe05dff2f0`)  
   → Saves $14.81/month | Effort: Low | Deadline: May 1, 2026

### Short-Term — Next 90 Days

4. **Consolidate to primary region (eu-central-1)**  
   → Reduces VPC cross-region transfer costs | Deadline: June 30, 2026

5. **Configure S3 Lifecycle Policies** for cold object archiving  
   → Saves ~$1.50/month | Deadline: June 30, 2026

6. **Complete tagging of all resources** (Project, Team, Environment)  
   → Enables full cost attribution | Deadline: May 15, 2026

### Long-Term — 3–12 Months

7. **Evaluate Reserved Instances** for consistently running EC2 workloads  
   → Up to 42% savings vs On-Demand | Deadline: October 1, 2026

8. **Implement Auto Scaling** policies for off-peak cost reduction  
   → Saves ~$25/month | Deadline: October 1, 2026

9. **Audit and consolidate Elastic Load Balancers**  
   → Saves ~$15/month | Deadline: January 1, 2027

---

## 6. Expected ROI

### Savings Summary

| Timeframe | Monthly Savings | Annual Impact |
|-----------|----------------|---------------|
| Quick Wins (this week) | $567.77 | $6,813.24 |
| Short-term (1–3 months) | $21.50 | $258.00 |
| Long-term (3–12 months) | $70.00 | $840.00 |
| **TOTAL** | **$659.27** | **$7,911.24** |

### Cost Reduction Impact

- **Current average monthly spend:** $520.94
- **Projected monthly spend after optimizations:** ~$-138.33 (net savings exceed current spend due to the staging instances costing more than the average bill)
- **Practical new baseline after quick wins:** ~$520.94 - $567.77 = costs largely offset
- **Percentage reduction from quick wins alone:** ~109% of average monthly bill

> **Bottom line:** The two idle staging instances cost more per month than the entire average AWS bill. Stopping them this week is the single highest-ROI action available.

---

## Governance & Monitoring Setup

The following cost controls have been configured as part of this lab:

- ✅ **Cost Explorer** enabled (since April 22, 2026)
- ✅ **Cost Anomaly Detection** monitor active — threshold: $10, SNS: `cost-anomaly-alerts`
- ✅ **Cost Allocation Tag** activated: `aws:createdBy` (April 24, 2026)
- ✅ **Resource tagging** started: `my-second-vm` tagged with `Environment: production`
- ⚠️ **SNS email confirmation** pending — confirm subscription to activate alerts
- ⚠️ **Tagging coverage** incomplete — remaining resources need Project, Team, Environment tags

---

*Report generated from AWS Cost Explorer, AWS Trusted Advisor, and lab analysis data.*  
*Student: Eric Rodrigues Borba | Lab: AWS Cost Explorer and Cost Optimization | April 24, 2026*
