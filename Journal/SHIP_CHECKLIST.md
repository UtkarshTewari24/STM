# Pre-shipping checklist

Use this immediately before submitting the project. Do not mark a box complete until the file or evidence is in the repository.

## Files

- [x] `BOM.csv` is in the repository root and includes purchase links.
- [x] PCB Gerber archives are in `PCB/`.
- [x] Desktop control software is in `Software/`.
- [ ] Add a full `.STEP` assembly to `CAD/`. It must show the complete build, including the electronics.
- [ ] Add the editable CAD source file alongside the `.STEP` export.
- [ ] Add the ESP32 controller firmware source to `Firmware/`.
- [ ] Add any firmware libraries, pin maps, and build instructions needed to reproduce the controller.

## README and evidence

- [ ] Write the project description and motivation in your own words.
- [ ] Add photos of the actual assembled project.
- [ ] Add a screenshot of the full 3D CAD assembly.
- [x] Include the existing wiring and circuit drawings.
- [ ] Add a BOM table to the end of the README.
- [ ] Ask someone for design feedback and record what changed.

## Final check

- [ ] Confirm every file path in the README and BOM works on GitHub.
- [ ] Confirm `git status` is clean after committing.
- [ ] Read the current Stardance shipping requirements once more before submitting.

This checklist cannot guarantee approval. It is here to catch the required files and evidence that the shipping guide asks for.
