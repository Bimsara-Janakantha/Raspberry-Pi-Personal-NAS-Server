# Mechanical Assembly

With all drives tested and prepared, everything was mounted securely inside the PC case.

---

## Steps

### 1. Open the Case

Remove both side panels and set them aside. Identify the 3.5-inch drive bays — these are where the HDDs will be mounted.

### 2. Mount the Hard Drives

Slide each HDD into a drive bay and secure with M3 or UNC 6-32 screws (usually included with the case).

**Airflow tip:** Leave at least one empty bay between drives wherever the bay layout allows. Packed drives trap heat and reduce drive lifespan.

Make sure the SATA connector end of each drive faces inward and is easily accessible for cable routing.

### 3. Mount the ATX PSU

Slide the ATX PSU into the rear PSU bracket and secure it with the four retaining screws. The PSU fan should face outward (toward the case's ventilation grille) or downward, depending on the case design.

### 4. Mount the Raspberry Pi

Choose a location for the Pi that has:

- Good airflow (not directly behind the PSU exhaust)
- Easy access to the SD card slot and USB ports
- Enough clearance for the PCIe ribbon cable to route cleanly to the Penta SATA Hat

Mount the Pi using standoffs, a 3D-printed bracket, or adhesive standoff pads. Ensure it is not resting directly on the metal case (this could cause a short).

### 5. Loose Cable Routing

Route cables loosely at this stage — don't zip-tie anything yet. Final cable management is done after all connections are made and tested.

---

## Layout Tips

```
[ Front of case ]
  ├── Drive Bay 1 — HDD 1 (4 TB SAS)
  ├── Drive Bay 2 — [empty gap for airflow]
  ├── Drive Bay 3 — HDD 2 (4 TB SAS)
  ├── Drive Bay 4 — [empty gap for airflow]
  ├── Drive Bay 5 — HDD 3 (4 TB SAS)
  └── Drive Bay 6 — HDD 4 (1 TB SATA)

[ Mid case ]
  └── Raspberry Pi 5 + Penta SATA Hat (on standoffs)

[ Rear of case ]
  └── ATX PSU
```

---

[← Drive Preparation](drive-preparation.md) | [Next: Drive Connectivity →](drive-connectivity.md)
