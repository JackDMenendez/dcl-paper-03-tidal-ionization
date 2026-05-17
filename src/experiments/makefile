# src/experiments/makefile -- per-experiment targets.
#
# Invoked from the repo root either via:
#   make experiments              (runs every target in EXPERIMENTS, slow)
#   make -C src/experiments exp_18_tidal_ionization
#
# Add a new experiment by:
#   1. dropping exp_NN_<name>.py and exp_NN_<name>.md alongside this file
#   2. appending the target name to EXPERIMENTS
#   3. adding a target rule below

RELATIVE_PATH := ../../
include $(RELATIVE_PATH)common.mak

EXPERIMENTS := exp_18_tidal_ionization

.PHONY: all $(EXPERIMENTS)

all: $(EXPERIMENTS)

# --- Individual experiments -------------------------------------------------
exp_18_tidal_ionization:
	$(PYTHON) exp_18_tidal_ionization.py
