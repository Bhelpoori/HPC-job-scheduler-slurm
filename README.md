# SLURM Usage and Best Practices Guide

This document provides an overview of common SLURM (Simple Linux Utility for Resource Management) commands and best practices for submitting, managing, and monitoring jobs on a shared compute cluster.

---

## 1. Submitting Jobs

### a. Batch Job Submission
Submit jobs using a SLURM job script (e.g., `job.slurm`):

```bash
sbatch job.slurm
```

Example job.slurm file:
```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --output=output_%j.log
#SBATCH --error=error_%j.log
#SBATCH --time=02:00:00
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --mem=8G

module load python/3.10
python my_script.py
```

b. Interactive Job

To get an interactive session on a compute node:
```
srun --time=01:00:00 --ntasks=1 --mem=4G --pty bash
```

---

## 2. Monitoring Jobs and Queue

a. View Job Queue

```
squeue
```

b. View Jobs by User

```
squeue -u $USER
```

c. View Detailed Job Information
```
scontrol show job <jobid>
```
d. View Pending Jobs and Reasons
```
squeue -t PD -o "%.18i %.9P %.8j %.8u %.2t %.10M %.6D %R"
```

---

## 3. Cancelling and Managing Jobs

a. Cancel a Job
```
scancel <jobid>
```
b. Cancel All Your Jobs
```
scancel -u $USER
```
c. Hold/Release a Job
```
scontrol hold <jobid>
scontrol release <jobid>
```

---

## 4. Checking Resource Usage and Availability

a. Cluster Overview
```
sinfo
```
Displays available partitions, node states, and time limits.

b. Detailed Node Usage
```
scontrol show node <nodename>
```
c. Summary of Node and CPU Usage
```
sinfo -N -l
```
d. Available Resources by Partition
```
sinfo -o "%P %a %l %D %C"
```
e. View Resource Utilization by User

If accounting is enabled:
```
sacct -u $USER
```
For summary of resource usage:
```
sacct -u $USER --format=JobID,JobName%30,Elapsed,AllocCPUS,State,ExitCode
```

---

## 5. Job Efficiency and Accounting

a. Job Efficiency Report
```
seff <jobid>
```
b. View Historical Job Records
```
sacct -j <jobid>
```

---

## 6. Best Practices
	1.	Specify Resource Limits Clearly
	•	Always define --time, --mem, and --ntasks to avoid over-allocation or job preemption.
	2.	Use Logs for Debugging
	•	Include --output and --error in job scripts to capture results.
	3.	Avoid Overloading the Scheduler
	•	Do not submit hundreds of tiny jobs individually; use job arrays:
```
sbatch --array=1-100 job.slurm
```

	4.	Check Queue Before Submitting
	•	Run sinfo or squeue to identify available partitions and load.
	5.	Use Modules
	•	Load required environments with module load.
	6.	Monitor Job Efficiency
	•	Use seff to evaluate CPU and memory utilization.
	7.	Cancel Unused Jobs
	•	Free up cluster resources using scancel when needed.

---

## 7. Common SLURM Commands Reference

Action	Command Submit job	
```
sbatch job.slurm
```
Submit interactive job	
```
srun --pty bash
```
View queue	
```
squeue
```
View your jobs	
```
squeue -u $USER
```
View partitions	
```
sinfo
```
Cancel job	
```
scancel <jobid>
```
Hold/Release job	
```scontrol hold/release <jobid>
```
View job details	
```
scontrol show job <jobid>
```
Check job efficiency	
```
seff <jobid>
```
Job history	
```
sacct -u $USER
```

---

## 8. Example: Job Lifecycle Workflow
	1.	Check available partitions and resources
```
sinfo
```

	2.	Submit a job
```
sbatch my_job.slurm
```

	3.	Monitor job status
```
squeue -u $USER
```

	4.	Inspect running job
```
scontrol show job <jobid>
```

	5.	Cancel or adjust job if needed
```
scancel <jobid>
```

	6.	After completion, check efficiency
```
seff <jobid>
```


---

## 9. Troubleshooting
	•	Job stuck in PD (Pending):
Run squeue -j <jobid> -o "%.18i %.9P %.8j %.8u %.2t %.10M %.6D %R" to check the reason.
	•	Job fails immediately: Check error_%j.log for details.
	•	Memory limit exceeded: Increase --mem in your script.
	•	Job exceeds wall time: Extend --time or optimize code.

---

## 10. Additional Resources
	•	SLURM Documentation: https://slurm.schedmd.com/documentation.html
	•	SLURM Quick Start Guide: https://slurm.schedmd.com/quickstart.html
	•	Cluster Administrator Contact: [Add your admin email or Slack channel]

⸻

Maintained by [Your Team Name] – Updated [October 2025]

---

> For access to all my prompts, get my Prompt Codex here: \
> 👉  Volume I: [Foundations of AI Dialogue and Cognitive Design](https://buymeacoffee.com/Marino25/e/398926)  
> 👉  Volume II: [Systems, Strategy & Specialized Agents](https://buymeacoffee.com/marino25/e/407285)  
> 👉  Volume III: [Deep Cognitive Interfaces and Transformational Prompts](https://buymeacoffee.com/marino25/e/408565)  
> 👉  Volume IV: [Agentic Archetypes and Transformative Systems](https://buymeacoffee.com/marino25/e/425929)  
> or, if you found this GPT useful, consider buying me a coffee here: 👉  [buymeacoffee.com/marino25](https://buymeacoffee.com/marino25)  
> Your support helps me keep building and sharing.
