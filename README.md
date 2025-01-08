# magnetic-survey
Code for an activity designed around the interpretation/analysis of magnetic data, in relation to some buried archaeological features.

Below is a minimal, self-contained Python script that illustrates how one might synthesize magnetic survey data over a 2D grid.  The script:

1. Sets up a regular 2D observation grid.  
2. Creates a collection of randomly located (and buried) point‐source anomalies (dipoles).  
3. Computes the total magnetic anomaly \(B_A\) at each grid point assuming an induced magnetization by a uniform background (earth’s) field \(\mathbf{B}_0\).  

> **Important Note**  
> In real geophysical modeling, one often measures a specific component of the magnetic field (often the vertical component \(B_z\) or the total field) and uses more sophisticated formulas for dipole fields, including directional effects.  For simplicity, this example shows how you might compute _just the vertical component_ of the induced anomaly from multiple dipole sources.  You can modify the code if you need other components or a full vector field.

---

## 1. Theoretical Background for a Single Dipole

If we assume all dipoles are induced by a uniform field \(\mathbf{B}_0\) pointing in the vertical direction (say along \(+z\)), then each anomaly can be approximated by a vertical dipole of moment

\[
\mathbf{m} = k \, V \, \mathbf{B}_0,
\]

where \(k\) is the magnetic susceptibility, and \(V\) is some effective volume of the point source.  In practice, you may just treat \(\mathbf{m}\) as a parameter that sets the strength of each anomaly.

The vertical component of the dipole field at a point \((x,y)\) on the surface (assuming the dipole center is at \((x_0, y_0, z_0)\) below the surface) is often written in textbooks as:

\[
B_{z,\text{dip}}(x,y) \;=\; \frac{\mu_0}{4\pi} \, \frac{m \left(2\Delta z^2 - \Delta x^2 - \Delta y^2 \right)}
{\left(\Delta x^2 + \Delta y^2 + \Delta z^2\right)^{5/2}},
\]

where
\[
\Delta x = x - x_0, 
\quad \Delta y = y - y_0,
\quad \Delta z = z_0
\]
(because the surface is at \(z=0\), and the dipole is buried at \(z=z_0>0\)).

When summing over multiple point dipoles, simply add the contributions from each dipole at each grid cell.

### How This Code Works

1. **dipole_vertical_anomaly(...)**  
   A helper function that, given a grid point \((x,y)\) and dipole parameters \((x_0, y_0, z_0, m)\), computes the vertical component of the anomaly.

2. **synthetic_magnetic_survey(...)**  
   - Creates a 2D grid of shape \((\text{ny}, \text{nx})\).  
   - Generates \(n_{\text{anomalies}}\) random dipoles, each with random \((x_0, y_0)\) within the grid bounds, a random depth \(z_0\), and a random dipole moment \(m\).  
   - Sums the contribution from each dipole at every grid node.  
   - Optionally adds a uniform background field \(B_0\).  

3. **Main script**  
   - Demonstrates how to call the survey function, print out the random dipole parameters, and visualize the results.

Feel free to adjust:
- The ranges used for depths (`z0`) and dipole strengths (`m`).  
- The measurement component (e.g., if you want total field anomaly instead of vertical only, or if you want to incorporate the inclination/declination of \(\mathbf{B}_0\)).  
- The units of your grid spacing and field magnitudes, depending on whether you prefer meters and nanotesla, or another consistent set.  

This code provides a straightforward illustration for creating synthetic data sets in magnetic surveying.
