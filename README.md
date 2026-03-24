# nsx-soc-hal

`nsx-soc-hal` is the stable NSX-facing umbrella target for SoC support.

It does not carry vendor source itself. Instead, it links the selected release-
specific wrapper modules:
- `nsx-ambiq-hal-r*`
- `nsx-ambiq-bsp-r*`

Applications and higher-level modules can depend on `nsx-soc-hal` without needing
to know which AmbiqSuite release-specific HAL and BSP modules were selected.
