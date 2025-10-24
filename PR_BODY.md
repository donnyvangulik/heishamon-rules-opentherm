# fix(dhw): let sterilization exceed 50 °C by disabling Quiet Mode and raising DHW pump-duty headroom

**Problem:** Sterilization stalled near 50 °C due to Quiet Mode throttling and conservative DHW pump-duty caps.

**Changes:**
- Quiet Mode forced to 0 while `@Sterilization_State == 1`.
- DHW pump-duty headroom set to 140 (final clamp 120–140).
- Keep CH dynamic cap and the `(@Pump_Speed / @Pump_Flow) > 145` safeguard.

**Test plan:**
1. Trigger sterilization.
2. Verify Quiet Mode → 0 during sterilization; MaxPumpDuty → 140 on DHW.
3. Confirm DHW exceeds 50 °C and cycle completes; values revert after sterilization.

**References:**
- HeishaMon: https://github.com/Egyras/HeishaMon
- Inspiration: gathering.tweakers.net (community threads)
- Base rules approach: https://github.com/blb4github/HeishaMon-Rules-with-Opentherm-Thermostat
