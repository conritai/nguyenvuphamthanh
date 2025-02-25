Provide your solution here:

# NGINX 99% Memory Usage Incidents

## Troubleshooting Steps

1. **Check System Resource Usage**  
   - **Command:** `htop` – Use this to check overall system resource usage.
2. **Identify Memory-Consuming Processes**  
   - **Command:** `ps aux --sort=-%mem | head -n 10` – This sorts and displays the top processes consuming memory.
3. **Check NGINX Status**  
   - **Command:** `systemctl status nginx` – Verify if NGINX is running properly.
4. **Inspect NGINX Logs**  
   - **Default Location:** `/var/log/nginx/access.log` or `/var/log/nginx/error.log`  
   - Use commands like `grep`, `tail`, or tools like `logtop` to check for request errors, unusual patterns, or high traffic activity.

---

## Potential Causes and Solutions

### High Traffic or DDoS Attack
- **Description:**  
  - Sudden traffic spikes, either due to legitimate high demand or malicious activity (e.g., from a single or multiple IPs).
- **Impact:**  
  - Memory exhaustion, degraded performance, or server downtime.
- **Recovery Steps:**  
  1. Begin with the **Troubleshooting Steps**. If monitoring services are configured, this can save time.
  2. Reconfigure NGINX to:  
     - Limit requests
     - Enable caching
     - Block malicious IPs manually
     - Increase connections and worker processes  
     - Reload NGINX to apply changes.
  3. Scale NGINX VMs vertically or horizontally (recommended) to maintain availability.
  4. Document malicious activity for future reference.
- **Solutions and Prevention:**  
  1. Consider services like Cloudflare or AWS Shield for enhanced protection.
  2. Use monitoring services like CloudWatch or Prometheus with Grafana for traffic monitoring and alerting.

---

### Caching and Buffer Issues Leading to Memory Leaks
- **Description:**  
  - Misconfigured caching or buffering can lead to memory exhaustion or outdated content being served.
- **Impact:**  
  - **Buffer Issues:**  
    - Memory exhaustion if buffers are too large.  
    - Truncated responses if buffers are too small.  
    - Slow loading times for large files.  
  - **Cache Issues:**  
    - High memory usage with moderate traffic.  
    - Inconsistent behavior due to full or corrupted cache storage.  
    - Outdated content being served.  
- **Recovery Steps:**  
  - **Buffer Issues:**  
    1. Begin with the **Troubleshooting Steps**.  
    2. Adjust NGINX buffer parameters appropriately.  
    3. Test and fine-tune buffer settings.  
  - **Cache Issues:**  
    1. Begin with the **Troubleshooting Steps**.  
    2. Configure appropriate `proxy_*` parameters for caching.  
    3. Verify cache files at `/data/nginx/cache/`.  
    4. Manually clear cache to check proper functionality.  
- **Solutions and Prevention:**  
  - Use monitoring tools like CloudWatch or Prometheus to track `$upstream_cache_status`.

---

### Handling Large File Uploads/Downloads
- **Description:**  
  - High resource consumption due to large file uploads or downloads.
- **Impact:**  
  - High memory and CPU usage leading to performance degradation or downtime.
- **Recovery Steps:**  
  1. Begin with the **Troubleshooting Steps**.  
  2. Limit request sizes to a suitable threshold.  
- **Solutions and Prevention:**  
  - Monitor file upload/download patterns using CloudWatch or Prometheus.

---

### Improper Worker Configuration Leading to Memory Leaks
- **Description:**  
  - Either too few or too many worker processes or connections can cause issues:  
    - **Too Few:** High CPU usage and request timeouts.  
    - **Too Many:** Excessive memory usage and potential OOM (Out of Memory) errors.  
- **Impact:**  
  - **Too Few:** Increased latency, 500 errors, or gateway timeouts.  
  - **Too Many:** OOM errors, CPU bottlenecks, server instability, or worker process termination.  
- **Recovery Steps:**  
  1. Begin with the **Troubleshooting Steps**.  
  2. For `worker_processes`: Check CPU cores using `grep -c ^processor /proc/cpuinfo` and set appropriately.  
  3. For `worker_connections`: Calculate `Max Clients = worker_processes * worker_connections` and ensure it's within limits.  
  4. Perform stress testing before redeployment.  
- **Solutions and Prevention:**  
  - Monitor worker performance using CloudWatch or Prometheus.

---

### Zombie Processes or Orphaned Worker Processes
- **Description:**  
  - **Zombie Processes:** Child process ends but parent fails to clean up.  
  - **Orphaned Worker Processes:** Parent process exits, leaving child processes unmanaged.  
- **Impact:**  
  - High CPU and memory usage leading to server instability and OOM errors.
- **Recovery Steps:**  
  1. Begin with the **Troubleshooting Steps**.  
  2. Manually clean up or restart NGINX.  
  3. Reconfigure NGINX to handle child signals correctly and set worker limits.  
  4. Consider systemd auto-restart for better recovery.  
- **Solutions and Prevention:**  
  - Monitor orphaned or zombie processes using CloudWatch or Prometheus.

---

### Insufficient or Incompatible Hardware Resources
- **Description:**  
  - Hardware resources are inadequate or incompatible with the NGINX version or workload.
- **Impact:**  
  - Consistent high memory and CPU usage, OOM errors, server crashes, or network bottlenecks.
- **Recovery Steps:**  
  1. Begin with the **Troubleshooting Steps**, adding:  
     - Disk I/O Analysis (`iostat`)  
     - Network Performance Monitoring (`ifstat`)  
  2. Apply temporary solutions:  
     - Clear Cache  
     - Stop unnecessary processes  
     - Restart NGINX to release resources  
  3. Scale NGINX VM vertically (if hardware is incompatible) or horizontally (recommended).  
  4. Conduct stress testing before redeployment.  
- **Solutions and Prevention:**  
  - Monitor hardware resource usage using CloudWatch or Prometheus.

---

## Notes
- Consistent monitoring and proactive configuration adjustments are crucial for maintaining NGINX performance.
- Always document incidents and recovery steps for future reference and faster resolution times.