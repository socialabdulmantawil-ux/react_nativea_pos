# Hardware Compatibility

**Status:** Not Yet Audited. No printer or scanner model has been identified or tested.

| Hardware | Exact model | Platform / connection | Compatibility evidence | Status |
|---|---|---|---|---|
| Thermal printer | Not Yet Audited | Not Yet Audited | None | Not tested |
| Barcode scanner | Not Yet Audited | Not Yet Audited | None | Not tested |
| QR scanner | Not Yet Audited | Not Yet Audited | None | Not tested |

## Original app findings

- Receipt preview uses browser `window.print()` with 80mm print CSS.
- No native thermal-printer SDK, Bluetooth/USB/network connection flow, barcode scanner, or QR scanner integration was found.
- Browser printing is not evidence that the owner's future tablet hardware will work.

## Owner input required

- Exact printer and scanner makes/models.
- Printer paper width and command set, where known.
- Required Android and iPadOS connection types.

## Validation checklist

- [ ] Verify native React Native and Expo Development Build compatibility.
- [ ] Verify Android and iPadOS support and required permissions.
- [ ] Check SDK availability, maintenance, and ESC/POS compatibility where applicable.
- [ ] Test connection reliability and printing/scanning on the selected hardware.

Do not select a hardware library before the actual models are known and compatibility is reviewed.
