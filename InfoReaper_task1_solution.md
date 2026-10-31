# Task 1 Analysis Report: FITS Moving Object Detection & Astrometry Pipeline
**Team:** InfoReaper  

---

## 1. Methodology & Pipeline Overview
Our pipeline processes sequential FITS astronomical images to detect and track moving sources while filtering out background noise and detector artifacts.

1. **Data Ingestion & Calibration:** Loaded raw FITS image data, cast pixel intensities to standard floating-point arrays, and normalized sensor baselines.
2. **Median Background Construction:** Generated a static sky reference template (`median_reference.png`) by taking the pixel-wise median across all temporal frames.
3. **Difference Imaging:** Subtracted the median reference frame from each raw frame to isolate transient signals from static celestial backgrounds.
4. **Candidate Source Detection & Kinematic Filtering:** Applied a $5\sigma$ thresholding rule to identify candidate detections. Evaluated source positions across consecutive frames using spatial bounds and kinematic constraints to eliminate non-moving noise spikes.
5. **Trajectory Reconstruction:** Tracked the target's centroid $(x, y)$ coordinates over time to construct a linear trajectory model (`InfoReaper_trajectory.png`).

---

## 2. Statistical Findings & Validation Metrics

* **Average Background Noise Level ($\sigma$):** $2.97$ pixel intensity units.
* **Detection Threshold ($5\sigma$):** $14.86$ pixel intensity units.
* **Average Peak Signal Strength:** $29.27$ pixel intensity units.
* **Signal-to-Noise Ratio (SNR):** $9.84\text{x}$.

---

## 3. Conclusion
The high SNR ($9.84\text{x}$) confirms that the moving source is a genuine astronomical signal rather than background fluctuation. The steady progression of pixel coordinates over time demonstrates a consistent velocity vector across the sensor plane.
