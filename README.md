# planar-exo-control-mujoco

Control of a 2-DOF planar exoskeleton leg (hip + knee) using three different strategies, validated through MuJoCo simulation.

Each notebook is self-contained: symbolic dynamics (SymPy) → linearization → controller synthesis → MuJoCo simulation → plots and video export.

juanpa/feature/actividad_sociotecnica


https://github.com/user-attachments/assets/79a20d2b-b83f-4201-9b42-ac145c7d63c9


## Physical model

The exoskeleton hangs vertically from a fixed base. The thigh (3.3 kg, 0.475 m) and shank (1.1 kg, 0.450 m) are actuated by hip and knee motors (±50 Nm). Gait is tracked against smooth reference trajectories inspired by healthy walking.

| Parameter | Value |
|---|---|
| Thigh mass | 3.3 kg |
| Shank mass | 1.1 kg |
| Thigh length | 0.475 m |
| Shank length | 0.450 m |
| CoM to hip | 0.360 m |
| CoM to knee | 0.150 m |

## Control strategies

| Notebook | Strategy | Approach |
|---|---|---|
| `Notebooks/Control_PID/control_PID.ipynb` | PID | Independent-joint SISO, second-order approximation, critically damped (ζ = 1) |
| `Notebooks/Controlador_Frecuencia/Control_Frecuencia.ipynb` | Frequency | Transfer function in frequency domain, closed-loop compensation |
| `Notebooks/Sistema_Variables_Estados/control_VE.ipynb` | State-space | Pole placement on the linearized 4‑state model (θ_h, θ_k, ω_h, ω_k) |

## Repository structure

```
Notebooks/
  Control_PID/control_PID.ipynb
  Controlador_Frecuencia/Control_Frecuencia.ipynb
  Sistema_Variables_Estados/control_VE.ipynb
Material_Complementario/       # Tutorials and reference PDFs
Material_Actividades/          # Deliverables (reports, models, socio-technical)
Resultados/                    # Simulation outputs
  Control_PID/
  Controlador_Frecuencia/
  Sistema_Variables_Estados/
requirements.txt
venv/
```

There are no `.py` modules — every notebook is a self-contained pipeline.

## Setup and run

### Local

```sh
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

Open any notebook in `Notebooks/` and run all cells. Each notebook installs no extra packages and assumes the venv kernel.

### Google Colab

1. Upload the notebook to [colab.research.google.com](https://colab.research.google.com).
2. At the top of the notebook, insert a code cell and run:
   ```python
   !pip install mujoco control mediapy sympy
   ```
3. Run all cells.

> **Note**: MuJoCo rendering in Colab requires `mediapy.show_video()` (already used in each notebook). The simulations will run headlessly and produce an embedded video.

## Dependencies

- `mujoco>=3.0` — physics simulation
- `control>=0.9.4` — control systems library (`import control as ct`)
- `sympy>=1.14` — symbolic dynamics derivation
- `numpy`, `scipy` — numerics
- `matplotlib` — plots
- `mediapy` — video rendering in notebooks

## Outputs

Results are saved to `Resultados/<ControlStrategy>/`:
- `MuJoCo_marcha_*.mp4` — gait animation
- `*.png` — reference tracking and torque plots
- `Metricas_*.txt` — RMSE and peak torque metrics