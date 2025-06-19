# Calibration on Geodetic Mass Balance (Original Calibration)

In this calibration, the target is the **geodetic glacier mass balance** over a certain time period. For GloGEM, the default data set is **Hugonnet et al. (2021)** for the period **2000–2020** and RGIv6.

The calibration process involves looping through **three calibration phases** in sequence:

1. **Calibration Phase 1: `cprec`** — Precipitation correction factor  
2. **Calibration Phase 2: `ddfsnow`** — Melt factor for snow (ddfice is defined as 2x ddfsnow)  
3. **Calibration Phase 3: `toff`** — Temperature offset (bias)

Each glacier is calibrated individually using the reanalysis product specified in the `input.pro` file. Parameter boundaries can be defined either in `input.pro` or by referring to the **reanalysis-specific** `regional_parameter` file.

---

## Calibration Phases

### Phase 1: Calibrate `cprec`

- The default range is typically **[0.8 – 2.5]**.
- Calibration starts with a fixed `ddf_snow = 3` (`ddf_ice = 2 × ddf_snow`).
- If the geodetic mass balance target is met with some combination of `cprec` and `ddf = 3`, the calibration ends here.
- If not, move to Phase 2.
- When transitioning to Phase 2, `cprec` is fixed at one of the **boundary values**, depending on which side is closest to the best-fitting value from Phase 1.

### Phase 2: Calibrate `ddf`

- `ddf` is now varied freely within its defined range (default in GloGEM: **[1.5 – 4.5]**).
- `cprec` remains fixed at a boundary value.
- If the target is still not matched, the process proceeds to Phase 3.

### Phase 3: Calibrate `toff`

- This phase adjusts the **temperature time series** using an offset.
- Only performed if both `cprec` and `ddf` fail to yield a sufficient match with the geodetic target.

---

## Summary
In the end the calibration ends up with a paramter set, that can be one of combinations indicated in the Figure below. 

![Conceptual sketch of the calibration loop for the calibration aimed towards the geodetic mass balance. Parameter ranges in the figure are the default boundaries that can be adjusted](images/original_calibration_scheme.png)

Figure: Conceptual sketch of the calibration loop for the calibration aimed towards the geodetic mass balance. Parameter ranges in the figure are the default boundaries that can be adjusted.
