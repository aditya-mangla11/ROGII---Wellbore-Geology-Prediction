## Description

Roughly 10,000 horizontal wells are drilled worldwide every year, yet much of the drilling process still relies on manual interpretation by experts. These operations require immense technical precision, where even small deviations from the target zone can lead to significant resource waste. If the well drifts into less favorable geology, it results in inefficient energy recovery and may require additional corrective measures that increase the overall environmental footprint of the site.

Interpreting the subsurface is challenging because direct measurements are inherently limited. Data from wells, seismic surveys, and logging tools only show part of the picture. Rock layers start out stacked like a layer cake, but can bend or break along faults, making it hard to know exactly where the drill bit sits within the formation. Geologists and engineers analyze incoming data to steer the well, but current analytical tools often struggle to match the nuance of expert interpretation.

In this competition, you'll develop machine learning models that predict the geology encountered along a horizontal wellbore. Your models should identify favorable layers from drilling data and help guide well placement more accurately during operations.

Your solution could help reduce resource waste by minimizing redundant drilling, improve operational safety by better predicting geological hazards, and move the industry toward automated systems that make faster, more consistent, and data-driven decisions.

A clearer map beneath the surface could make every meter count.

---

## Evaluation

Submissions are scored on the **root mean squared error**. RMSE is defined as:

$$\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n}(y_i - \hat{y}_i)^2}$$

where $\hat{y}$ is the predicted value, $y$ is the original value, and $n$ is the number of rows in the test data.

---

## Dataset Description

The competition data comprises horizontal well trajectories and vertical reference logs (Typewells) used for geological prediction. Your goal is to predict the `TVT` (True Vertical Thickness) for the evaluation zone of each horizontal well.

The data is organized into `train/` and `test/` directories, where each well is identified by a unique 8-character hash (e.g., `015fe0d2`).

---

### File and Field Information

- **`train/`** Contains the training data. Each well has three associated files:

  - **`{WELLNAME}__horizontal_well.csv`** - Trajectory, geological surfaces, and log data.
    - `WELLNAME` - unique identifier for the well.
    - `MD` - Measured Depth (ft): The total length of the wellbore from the surface.
    - `X` - Easting (ft): Spatial coordinate in the horizontal plane.
    - `Y` - Northing (ft): Spatial coordinate in the horizontal plane.
    - `Z` - True Vertical Depth (ft): The vertical distance below sea level.
    - `ANCC`, `ASTNU`, `ASTNL`, `EGFDU`, `EGFDL`, `BUDA` - Predicted depth of various geological formations (Training only).
    - `TVT` - True Vertical Thickness (ft): The manually interpreted geological position for each 1 ft of the lateral well. This is the target variable (Training only).
    - `GR` - Gamma Ray (API): Log measuring natural radioactivity of the rock.
    - `TVT_input` - Input Target (ft): A copy of `TVT` provided as a feature. This column contains `NaN` values for the evaluation zone.

  - **`{WELLNAME}__typewell.csv`** - Vertical reference log for geological correlation.
    - `TVT` - Vertical Depth Index (ft): Primary depth reference for the vertical log. Corresponds to TVT (geological position) of the associated horizontal well.
    - `GR` - Gamma Ray (API): The vertical Gamma Ray signature used for correlation.
    - `Geology` - Formation Label: Categorical label indicating the geological unit (e.g., `EGFDL`, `BUDA`).

  - **`{WELLNAME}.png`** - Visualization of the well path and geological cross-section.

- **`test/`** Contains the evaluation data for about 200 wells. Each well has two associated files:

  - **`{WELLNAME}__horizontal_well.csv`** - Trajectory and log data. In these files, the `TVT` target is hidden (replaced with `NaN`) in the evaluation zone.
  - **`{WELLNAME}__typewell.csv`** - Vertical reference log for the test well.

> **Note:** The `test/` folder visible here contains only a few instances from the training set as example data to help you author your submissions. When your submission is rerun on the hidden test set, these will be replaced with the actual test data.

- **`sample_submission.csv`** - A sample submission file in the correct format.
  - `id` - A unique identifier for each prediction point, formatted as `{WELLNAME}_{row_index}` (e.g., `015fe0d2_1654`).
  - `tvt` - Your predicted True Vertical Thickness (ft).
